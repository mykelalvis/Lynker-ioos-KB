---
date created: 2024-10-02 12:51
date updated: 2024-10-02 12:53
---

# Build Volume Creation

The build system, in the guise of the [[BuildExecutor]], generates quite a few filesystems.  Each of these are frequently, but not always, disposable.  The volumes serve as scratch space for processes or durable storage for artifacts and results.

A build volume some block-storage system that can be mounted to some instance of compute for use within the build process.  It must be somewhat durable, in that it cannot simply disappear with the compute instance.  How this volume is generated is defined by the provider specified within the [[BuildExecutor]].  Currently, there are providers [[#AWS]].

A build volume is expected to be created formatted and empty, ready to be mounted.

## AWS

The AWS provider executes calls on behalf of the build system into  Amazon Web Services.  For the creation of build volumes, the provider will take the following information

|  Input | Description                       |
| :----: | --------------------------------- |
|  `az`  | Availability Zone within a region |
| `size` | Required Size in Gb               |

It will return the following information

| Output | Description                           |
| :----: | ------------------------------------- |
|  `id`  | ARN of a formatted blank filesystem   |
| `size` | Human readable size in Gb per `df -h` |


