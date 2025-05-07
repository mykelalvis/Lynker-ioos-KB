---
date created: 2024-10-02 12:24
date updated: 2024-10-02 13:02
tags:
  - '#TODO'
---

# Build Executor

The BuildExecutor is the supervisory process that takes a full interpolated [[Builder Specification|buildspec]] from a [[Builder Specification Interpolator]] and attempts to produce a "clean room" artifact that is the [[Executable Model Package|EMP]] that can be provided to some [[environment]] that can take externally provided data and produce "results".

Primarily, the BuildExecutor is the source of identities and filesystems upon which a [[BuildStep]] (and its [[BuildSubStep]]s) is performed, and is responsible for capturing the results of each [[BuildStep]].

During the course of execution, the BuildExecutor will execute a number of heavyweight process sub-shells as part of the Build execution.

These subshells will have a number of environment variables set in them based on various aspects of the buildspec.

At several points, a crypting string will be present that says
`export BUILD_FAIL_STATE=something_or_another`
and possibly 
`export BUILD_FAIL_STATE_id=some_value`

This command should be run at that point in the build to enable recovery during [[#Build Failure]]

## Filesystems

### Root Filesystem

The BuildExecutor operates with a known "root filesystem", referred to as the "build root".

For any given step within the process, the location of the "build root" remains constant and is made available in the exported environment variable `BUILD_FS_ROOT` as an absolute path.

Each [[BuildStep]] has a [[#Step Filesystem]] attached to it.  This may or may not be a _new_ filesystem, depending on the configuration of the spec.

#### Identity

`export BUILD_FAIL_STATE=initial_fs`

The Build executor performs [[Build Volume Creation]] based on the expected needs of the system.  This is expected to be very minimal, as most other things are handled with additional filesystems.  The placeholder for this filesystem is "provisional_root".
`export BUILD_FAIL_STATE_ID=${provisional_root_id}`

`export BUILD_FAIL_STATE=interpolation`

A BuildExecutor operates against a formatted, interpolated [[Builder Specification Interpolator|BSI]]-provided YAML document we will call the "buildspec".

- The buildspec is formatted according to some well-defined specification that is expected to never change
- The formatted YAML is then converted to a formatted JSON string
- The string value of that JSON string is converted to bytes, which are then fed into a SHA-512 checksum
- The SHA-512 is then converted to a Type-3 UUID using the bytes of the SHA-512, which is the `BUILD_ID`
- The environment variable `BUILD_FS_ROOT=${HOME}/${BUILD_ID}`

`export BUILD_FAIL_STATE=mount_initial_fs`

- The provisional_root filesystem is mounted at `${BUILD_FS_ROOT}`

`export BUILD_FAIL_STATE=mounted_initial_fs`

- The formatted YAM is written to `${BUILD_FS_ROOT}/.buildspec.yaml`
- The fomatted JSON is written to `${BUILD_FS_ROOT}/.buildspec.json` using 2-space indentation for whitespace.
- The SHA-512 is written to `${BUILD_FS_ROOT}/.buildspec.sha512`
- The `${BUILD_ID}` is written to `${BUILD_FS_ROOT}/.buildspec.uuid`

```
pushd ${BUILD_FS_ROOT}
export BUILD_FAIL_STATE
touch .gitignore
touch .build.filesystems
mkdir sources && chmod 700 sources
mkdir build && chmod 700 build
popd

```

At this point, the `${BUILD_FS_ROOT` should have 5 files and 2 directories.

### Source Filesystem

`export BUILD_FAIL_STATE=source_fs`

Obtain a "build source filesystem" of a specified size. 
Append the returned id of the filesystem to `${BUILD_FS_ROOT}/.build.filesystems`

`export BUILD_FAIL_STATE=source_fs_obtained`

The source for a given build is cloned into the "build source filesystem", which is indicated by `${BUILD_FS_SOURCE}`.  This is a separate filesystem that is built and has whatever sources cloned/copied onto it.

#### Mount Sources FS Writeable

Mount the filesystem at `${BUILD_FS_SOURCE}` as a writeable FS

`export BUILD_FAIL_STATE=source_fs_mounted`



#### Clone Sources

`export BUILD_FAIL_STATE=cloning_sources`

#TODO 

#### Remount read-only

`export BUILD_FAIL_STATE_cloned_sources`

`BUILD_FS_SOURCE` is set to `${BUILD_FS_ROOT}/sources`.  

Unmount the source filesystem
`export BUILD_FAIL_STATE=source_fs_unmounted`
Remount the source filesystem as read-only.
`export BUILD_FAIL_STATE=source_fs_remounted`

### Audit Filesystem

The BuildExecutor produces a filesystem that is an "audit-log" of the build process.  This has the potential for making it easier to debug builds.

An environment variable of `BUILD_FS_AUDIT=${HOME}/${BUILD_ID}_audit` is set

The BuildExecutor performs [[Build Volume Creation]] on a filesystem that is twice the size of the largest filesystem required for the build
`export BUILD_FAIL_STATE=audit_fs_created`
This filesystem gets mounted at `${BUILD_FS_AUDIT}`
`export BUILD_FAIL_STATE=audit_fs_mounted`
The following commands are performed on that filesystem:
```
pushd ${BUILD_FS_AUDIT}
git init
echo "# ${BUILD_ID}" > README.md
date >> README.md
rsync -az --exclude=.git ${BUILD_FS_ROOT} .
ls -lR . > .build.fileslist.txt
git add .
git commit -m "Initialize for ${BUILD_ID}"
git branch -M ${BUILD_ID} # Produce a branch that is this BUILD_ID
git tag -a -m "${BUILD_ID}.START"
```
`export BUILD_FAIL_STATE=audit_fs_committed`
If the build supplies a `retain.remote_repo` map
- If `retain.remote_repo.uri` starts with `git`, then the following is done
```
    #TODO Somehow extract an ssh key or credentials
    git remote add origin ${retain.remote_repo.url}
    git push -u origin ${BUILD_ID}
```
- If `retain.remote_repo.url` is an `s3` URI (i.e. starts with `s3:`)
        - The target `s3` URI is checked for accessibility
            - if inaccessible, this is considered a [[#Build Failure]]
            - if it is not empty, this is considered a [[#Build Failure]]  #TODO Fix this
        - `BUILD_S3_URI` is set to `${retain.remote_repo_url}`


- finally, execute the following
```
    if [ "$(git remote)" == "origin" ]; then  git push; fi
    if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync --delete . ${BUILD_S3_URI}; fi
    popd
```

This provides the system with a commit and a tag that is the starting state of the filesystem, after any source code checkouts and before any of the build steps are done.

## Steps 

Builds have effectively 4 phases that execute in-order
- [[#Dependency Resolution]]
- [[#Pre Actions]]
- [[#BuildSteps]]
- [[#Post Actions]]

### Dependency Resolution

#TODO
Dependency resolution is a primary part of all builds.  The expectation is that prior to any step occurring, it is necessary to resolve any dependency that the build itself might have.

### Pre Actions

#TODO
Pre-actions, and the equivalent post action, are afterthoughts to an ordered process.  These are elements that we expect someone to place in a parent spec rather than in their own.

A pre-action is essentially a build step that does not transport its build filesystem to the [[BuildStep]]s.

### BuildSteps

#### Step Filesystem

Each [[BuildStep]] within the build has its own (potentially unique) filesystem.  This is the "build step filesystem", and is at `${BUILD_STEP_ROOT}` which is set to `${BUILD_FS_ROOT}/build`

Prior to the execution of each step, and according to the buildspec, the BuildExecutor will either [[#Retrieve an Existing Build Step Filesystem]] or [[#Create a Build Step Filesystem]].  In any case, if a filesystem is created or retrieved, the BuildExecutor will [[#Mount the Build Step Filesystem]]

If a build step filesystem cannot be acquired through the following steps, this is considered a [[#Build Failure]]

##### Retrieve an Existing Build Step Filesystem

At the end of a [[BuildStep]], the BuildExecutor sets the `BUILD_STEP_FILESYSTEM_IDENTIFIER_PREVIOUS` to the value of `${BUILD_STEP_FILESYSTEM_IDENTIFIER}`, which is then passed on to the next build step.

A given build step filesystem is defined by its `BUILD_STEP_FILESYSTEM_IDENTIFIER`.  This identifier should be enough information for the BuildExecutor to obtain the filesystem in question.   For instance, the identifier for an AWS EBS volume (one of the currently-most-likely provider candidates) is the ARN of the EBS volume in question.

If retrieval is specified and there is no previous filesystem (i.e. it is the first step), then the BuildExecutor will attempt to [[#Create a Build Step Filesystem]] instead and supply that to the step.

##### Create a Build Step Filesystem

A volume is [[Build Volume Creation|created from scratch]], per the BuildExecutor's provider and the `BUILD_STEP_FILESYSTEM_IDENTIFIER` is set to the returned id.

##### Mount the Build Step Filesystem

The current build step filesystem is mounted at `${BUILD_FS_ROOT}/build` by the BuildExecutor using its provider and `${BUILD_STEP_FILESYSTEM_IDENTIFIER}`

Once a build step filesystem is mounted, [[#Identity|the build information]] is read and the configuration for this particular [[BuildStep]] is extracted

Much like is done for the [[#Root Filesystem]], the build step information is written to the filesystem
- The value `${BUILD_STEP_ID}=${BUILD_STEP_FILESYSTEM_IDENTIFIER}` is appended to `${BUILD_FS_ROOT}/.buildstep.filesystems`
- The build step config from the buildspec is written to the filesystem as `${BUILD_STEP_ROOT}/.buildstep.yaml`
- That YAML is converted to JSON and written as `${BUILD_STEP_ROOT}/.buildstep.json`
- Both of these files can and will over-write existing files with the same name

##### Save the existing Build Step Filesystem State

The BuildExecutor will, at this point, do the following:

```

export BUILD_TAG=${BUILD_STEP_ID}.START # This is a function we will build into the shell
pushd ${BUILD_FS_ROOT}
rm -rf build
mkdir build
chmod 700 build
rsync -az --exclude=.git ${BUILD_FS_ROOT} .
ls -lR . > .build.fileslist.txt
git add .
git commit -m "Beginning of ${BUILD_STEP_ID}"
git tag -a -m "${BUILD_TAG}" ${BUILD_TAG}
if [ "$(git remote)" == "origin" ]; then  git push; fi
if [ "${BUILD_S3_URI}" != "" ]; then aws s3 sync --delete . ${BUILD_S3_URI}; fi
popd

```

### Post Actions

## Environment Variables

- The system will set or override the following environment variables:
  - `BUILD_ID` # UUID of the SHA-512 of the whole document
  - `BUILD_FS_ROOT` # Absolute path to the clean mounted build filesystem root for this build
  - `BUILD_FS_SOURCE` # Absolute path to a read-only mount of any checked-out code for this build
  - `BUILD_FS_AUDIT` # Absolute path to the git repo copy of the BUILD_FS_ROOT with commits at steps
  - `BUILD_STEP_ID` # Name of the step in the buildspec dictionary
  - `BUILD_STEP_NUMBER` # Incremented number of the step
  - `BUILD_SUBSTEP_ID` # Name of the substep in the dictionary
  - `BUILD_STEP_FILESYSTEM_IDENTIFIER` # The provider-specific identifier for a filesystem for this step
  - `BUILD_STEP_FILESYSTEM_IDENTIFIER_PREVIOUS`  # the provider-specific identifier for the filesystem for the previous step
  - `BUILD_S3_URI` # Possibly set as the target for replication of the audit FS
  - `BUILD_FAIL_STATE` # enumeration of possible states for a build to fail
  - `BUILD_FAIL_STATE_ID` # Step/substep/action/etc during which a failure occurred, to enable cleanup
  - `${BUILD_SUBSTEP_ID}.START` # timestamp that this substep was started
  - `${BUILD_SUBSTEP_ID}.RECOVER` # timestamp that this substep recovery was started
  - `${BUILD_SUBSTEP_ID}.RECOVER_CODE` # The exit code of the original task that forced a recovery
  - `${BUILD_SUBSTEP_ID}.END` # timestamp that this substep ended
  - `${BUILD_SUBSTEP_ID}.SKIP`  # timestamp that this substep was skipped
  - `${BUILD_SUBSTEP_ID}.STATUS` # exit statis of this substep



> [!warning] `BUILD_` and UUID env vars
> As a general rule, it is advisable to not set any environment variable that begins with `BUILD_` in any sub-executable, like a task or condition, or to use any variable that starts with a UUID.


## Build Failure

Any given build may fail for any number of reasons.

However, as the build system generates a lot of transient information, it is important to understand when the failure occurred.

Cases for `${BUILD_FAIL_STATE}` are as follows.  As a rule, every _previous_ step must be performed once the failure cleanup is done for a givens step

### `initial_fs`

Remove filesystem found in `${BUILD_FAIL_STATE_ID}` if possible

### `interpolation`

nop

### `mount_initial_fs`

nop


### `mounted_initial_fs`

Unmount initial FS at `${BUILD_FS_ROOT}` if possible 


### `source_fs`

nop

### `source_fs_obtained`

remove all files in `${BUILD_ROOT_FS}/.build.filesystems`

### `source_fs_mounted`

`umount ${BUILD_FS_SOURCE}`