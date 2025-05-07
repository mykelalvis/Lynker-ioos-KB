A BuildSubStep is where the "real work" gets done.

The format of a BuildSubStep

```yaml
# ....
substeps:
    - stepname1:
        # Optional means that if that BuildSubStep fails, it does not fail the BuildStep
        optional: false
        condition: EXECUTABLE_ACTION_A
        task: EXECUTABLE_ACTION_B
        recover: EXECUTABLE_ACTION_C
    - stepname2:
        optional: true
        condition: EXECUTABLE_ACTION_1
        task: EXECUTABLE_ACTION_2
        recover: EXECUTABLE_ACTION_3
    - thirdstep: EXECUTABLE_ACTION_Z
```

`substeps` of a [[BuildStep]] are a dictionary.  In the above example, the keys `stepname1`, `stepname2`, and `thirdstep` are the `BUILD**_SUBSTEP_NAME**` of the BuildSubSteps.  These must be unique within the containing [[BuildStep]], per the rules of a YAML dictionary.

Note that if the value of a build step name is a string instead of a dictionary, like `thirdstep` above, then that _**string is interpreted as a dictionary**_ with the `task:` element (see below) with no other elements set _**except for upstream inherited ones**_.  This means that such a task is a shell command.


- Each BuildSubStep is an indexed/ordered element within a [[BuildStep]], called it's "step number"
- Each step number is injected into the system as `${BUILD_STEP_NUMBER}`, a zero-padded integer starting with `0000` and proceeding to `9999`.
    - `0000` is the first element of the list, `0001` is the second,  etc.
- Each BuildSubStep is uniquely identified as  `BUILD_SUBSTEP_ID=${BUILD_STEP_GUID}.${BUILD_SUBSTEP_NAME}`
- During the initialization of the execution of a BuildSubStep, 
    - the `${BUILD_SUBSTEP_ID}` will be appended to the file `./.buildsubsteps`
    - the value of `"${BUILD_SUBSTEP_ID}.STATUS"` will be unset
    - the value of `"${BUILD_SUBSTEP_ID}.START"` will be set to the local timestamp for the machine
- A BuildSubstep may have an `condition:` execution test value.  This will be a test that returns 0 or non-zero.  
    - If non-zero
        - the BuildSubStep _will not execute_ 
        - `"${BUILD_SUBSTEP_ID}.SKIP"` will be set to the local timestamp _at the moment of skipping_
        - `"${BUILD_SUBSTEP_ID}.STATUS"` will be set to `0` (i.e. "successful")
    - If it returns 0, then the BuildSubStep _will attempt to execute the task_ as below
        - A BuildSubStep is a single [[BuildTask]] that passes or fails according to POSIX process execution return code values.  Specifically, a 0 is a success, and non-zero is some form of failure.
        - Upon completion or failure, the value of `"${BUILD_SUBSTEP_ID}.STATUS"` is set to the return code
        - If the BuildSubStep fails (i.e. the return code is non-zero), a BuildSubStep may possibly have defined a recovery process.
            - If a recovery process is defined, then the recovery process is executed once the original process fails after the `.STATUS` value has been set
            - `"${BUILD_SUBSTEP_ID}.RECOVER"` is set to the local timestamp
            - `"${BUILD_SUBSTEP_ID}.STATUS"` will be set to the return code of the recovery process
- Check `"${BUILD_SUBSTEP_ID}.STATUS"`
    - If the value is `0`, then the process is considered a success.
    - If it is not `0`, then 
        - If the BuildSubStep is set to `optional: true` : 
            - This BuildSubStep is _considered_ "successful" (_even though it failed_).  It's value remains whatever the status was actually set to.
        - If the BuildSupStep is not set to `optional: true` :
            - the process is assumed to have failed
            - the BuildSubStep has failed, and this the BuildStep has failed
            - the overall build itself has failed (build's cannot denote that arbitrary steps are allowed to fail, only a substep)
- `"${BUILD_SUBSTEP_ID}.END"` is set to the local timestamp
- The system _will_ preserve these variables exported during processing, so downstream [[BuildSubStep]]s can determine the recovery value of a previous BuildSubStep
