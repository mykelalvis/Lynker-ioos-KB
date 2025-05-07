A BuildStep is an atomically transactional activity that must successfully complete in order for the process of producing a [[Compiled Executable Data Package|CEDP]] to complete.

A BuildStep is provided with a number of environment variables from the [[BuildExecutor]]

## Identity

For every task performed within a BuildStep, the [[BuildExecutor]] will set some variables

### `BUILD_ID`


### `BUILD_FS_ROOT`

## Source-code Managed Per-Step Filesystem

The [[BuildExecutor]] creates a (separate) filesystem for execution of the build.

This filesystem is:
- twice the size of the base build system, by default, and formatted
- mounted at `/${BUILD_ID}`

### SCM Per-Step Initialization
- execute the following
 ```
pushd /${BUILD_ID}
touch ./.gitignore
echo "${BUILD_ID}" > .BUILD_ID
git init
git add .
git commit -m "Initialize for ${BUILD_ID}"
git branch -M ${BUILD_ID} # Produce a branch that is this build ID
```

- If the build supplies a `retain.remote_repo` map
    - If `retain.remote_repo.uri` starts with `git`, then the following is done
    ```
    #TODO Somehow extract an ssh key or credentials
    git remote add origin ${retain.remote_repo.url}
    git push -u origin ${BUILD_ID}
    ```
    - If `retain.remote_repo.url` is an `s3` URI (i.e. starts with `s3:`)
        - The target `s3` URI is checked for accessibility
        - `BUILD_S3_URI` is set to `${retain.remote_repo_url`

- execute the following
```
popd
```

### Pre-Step Processing
Execute [[#Sync Build FS]]

```
pushd /${BUILD_ID}
git add .
git commit -m "${BUILD_STEP_ID} start"
if [ "$(git remote)" == "origin" ]; then  git push; fi
if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync . ${BUILD_S3_URI}; fi
popd
```

Optionally, `git push` if remote repositories are used

### Pre-Substep Processing

```
pushd /${BUILD_ID}
git add .
git commit -m "${BUILD_SUBSTEP_ID} start"
if [ "$(git remote)" == "origin" ]; then  git push; fi
if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync . ${BUILD_S3_URI}; fi
popd
```

Optionally, `git push` if remote repositories are used
### Post-Substep Processing

```
pushd /${BUILD_ID}
git add .
git commit -m "${BUILD_SUBSTEP_ID} end"
if [ "$(git remote)" == "origin" ]; then  git push; fi
if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync . ${BUILD_S3_URI}; fi
popd
```

### Post-Step Processing

After every step, then following  is executed
```
pushd ${BUILD_FS_ROOT}
git add .
git commit -m "${BUILD_STEP_ID} end"
if [ "$(git remote)" == "origin" ]; then  git push; fi
if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync . ${BUILD_S3_URI}; fi
popd
```

### Sync Build FS

```
pushd ${BUILD_FS_ROOT}
rsync -az --delete --exclude=.git . /${BUILD_ID}`
popd
```
## Facts

The following facts are true during a BuildStep execution:
- A BuildStep itself is not an executed (heavyweight) operating-system process.  It is a series of [[BuildSubStep]]s.
- A BuildStep element is globally identified by a Type-3 generated UUID from the checksum of the formatted, fully-resolved configuration exported as a string.
    - This value is injected into the execution system (and all subprocesses) as `${BUILD_STEP_ID}`
- A single BuildStep consists of an ordered list of up to 10000 [[BuildSubStep]]s (`0000`-`9999`)
    - That is an arbitrary limit because 4 digit numbers are easier to read than 5 digit numbers
- A BuildStep's primary job is to:
    - Allocate a filesystem to perform the BuildStep on, if one was not already allocated
    - Be a "container" of [[BuildSubStep]]s for the [[BuildExecutor]] to execute
    - Manage the filesystem across various BuildSubSteps
- BuildStep configurations have several values in addition to the list of [[BuildSubStep]]s
  > [!warning] Downstream Inheritance
  > Any additional BuildStep configuration is carried over to the next BuildStep for the following types:
  > - `retain`
  > - `continuance`

    - The `retain` group is a dictionary with several subvalues.
    - if `retain.fs` is set to `minimal`
        - A GIT repository for the locally managed filesystem will be created at the start of the process, but no values will be rsync'd and then `git add`'d to the repo.
        - The initial (usually blank) initial filesystem will be tagged as `${BUILD_STEP_ID}.START`
        - On each step/iteration, any `.gitignore` file will be copied from the working tree
        - The following will  be prepended to any existing `.gitignore`, if not present
            - `.git`
        - A `git add .` will be performed at the root of the copied filesystem
        - Upon completion, it will contain at  least two tags
            - The first tag is called `${BUILD_SUBSTEP_ID}.START`, and is the state of the working filesystem prior to beginning the BuildStep
            - The second tag is called `${BUILD_SUBSTEP_ID}.END`, and is the state of the working filesystem at the end of the BuildStep
    - if `retain.fs` is set to `step`
        - It will also contain a tag with a given `${BUILD_SUBSTEP_ID}` which is a committed snapshot of the managed filesystem at the end of a given substeps retention  The last build step number will have the same hash as the `${BUILD_SUBSTEP_ID}.END` tag.
    - if `retain.fs` is set to `complete`
        - It will also contain a tag with a given `${BUILD_SUBSTEP_ID}.recover`. which is a snapshot of the managed filesystem at the end of  the step number's primary process failure
    - If `retain.log`
        - is set to `true`
            - a BuildSubStep will retain
                - a `BuildStep.${BUILD_SUBSTEP_ID}/out` file which is the logged stdout of the given BuildSubStep.
                - a `BuildStep.${BUILD_SUBSTEP_ID}/err` file which is the logged stderr of the given BuildSubStep.
        - is set to `consolidate`,
            - then stderr will be redirected to stdout during execution and the output retained in the `(...)/out` file (no `err` file, or at least empty)
    - if `retain.data` is set to `basic`
        - The BuildSubStep will retain `BuildStep.${BUILD_STEP_ID}/data` which is any dataset that was handed to the BuildStep during the process.   Each BuildSubStep will be part of the commit in the event that `retain.fs` has been set to save data
      >     [!info] Note
      >     This is usually test or confirmation data.
      >     Configuration is performed more directly using the sources of the system.
      >     Processing data (i.e. "Real work") is handed to the compiled model in a totally different process.  This is just describing the BuildSteps, not the ExecutionSteps
    - if `retain.env` is set to `basic`
        - the BuildSubStep will retain a dump of environment variables to a local file `./.envrc.${BUILD_SUBSTEP_ID}`
    > [!warning] `retain` has consequences!
    > IF `retain` has any values set at all, (i.e. not unset), including inheriting the values from an upstream BuildStep, then some form of GIT repository has been created and will be retained by the system for the duration of the build.
    > At the end of the BuildStep processing, a `git add .` is performed at the root of the FS and committed as the tag `"${BUILD_STEP_ID}.END"`


    - `continuance` is a configuration group for transporting items to subsequent BuildSteps
        - If  set to `filesystem` the entire (final) filesystem of this build is used as the filesystem in the next BuildStep instead of being reset.
        - if  set to `discard`, the entire filesystem git repository is discarded prior to the start of this step. and then unset for subsequent BuildStep elements
        - if  set to `reset`, then the git repository is discarded and rebuilt.
    - `artifacts` is a configuration group for identifying artifacts generated during the build execution #TODO

Aside from a potentially continued filesystem from a previous BuildStep, a given BuildStep is entirely independent of any other BuildStep.

The [[BuildExecutor]] does the following upon a given processing of a BuildStep
- Obtain the local filesystem
    - All BuildSteps occur on a (usually independent) filesystem
    - If the previous BuildStep passed its filesystem to this BuildStep, then unless this step resets it, the filesystem will be as passed in.
        - otherwise, the filesystem should be empty.

- **INITIALLY**, a BuildStep
    - Generates a filesystem location or acquires a previous o
    - ensures that the file `./.buildsteps` present and empty.
    -
- Upon  completion of all BuildSubSteps,
    - the BuildStep is considered successful if all [[BuildSubStep]]s succeeded.
        - Any failed substep is considered a failure, and that is passed up to the [[BuildExecutor]]

A common BuildSubStep is to perform some task, like a compilation on some set of source code.  Frequently, in order for that compilation to complete, a given set of pre-requisites must be performed.
