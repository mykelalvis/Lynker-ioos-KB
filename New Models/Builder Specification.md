---
date created: 2024-09-24 14:39
aliases:
    - buildspec
---

A Model Builder Specification ("buildspec" hereafter) is a declarative document that describes all the necessary imperative steps that produce a "clean room" [[Compiled Executable Model|CEM]]

The buildspec itself is defined by an inheritance model that allows for pre-configuration of standard options within a "parent" arrangement while still providing the ability to override segments of the finally produced specification to be passed to a [[BuildExecutor]] for execution and ultimately, [[release]].  The inheritance/override aspects are handled entirely by the [[Builder Specification Interpolator]].

A buildspec is defined as follows:

- 0 or more dependencies
- 0 or more pre-build
- 1 or more [[BuildStep]]s
    - Each of which has 1 or more [[BuildSubStep]]s
- 0 or more post-build processors

Example:

```yaml
---
pre:
    - step1:
        shell: "echo starting"
    - step2:
        shell: "Seriously, we're starting"
    - step3:
buildSteps:
- bstep1:
    retain:
        fs: complete
    substeps:
        - step1.1:
            optional: true
            task:
                shell: "make clean"
        - step1.2:
            task:
                shell: "make"
                recover: "make clean && make" # Try re-cleaning
        - step1.3:
            

- step2ofthebuild:
    retain: # Is inherited from bstep1
    substeps:
    - steps2.1of2: "make install"
    - steps2.twoof2: 
        optional: true
        task:
            shell: "make verify"
            recover: "echo Verify didn't work"
        
```