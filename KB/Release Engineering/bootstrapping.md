---
date updated: '2021-04-28T09:29:30-05:00'
tags:
  - '#bootstrapping'

---

# Bootstrapping

Bootstrapping, in this context, is the process of using automation to setup further automation.

As example, in order to have `S3` buckets for keeping [[Terraform]] state, you need several things -- not the least of which is somewhere to keep the remote state in.  That's probably a bucket.

However, creating the bucket by hand _also_ has some problems.

Thus, you might write some automation which stores its state _locally_.  This produces a bucket, which can then be used to store subsequent state _remotely_.  One might consider the bootstrap action as a [[dependency]].

The first automation is the "bootstrap", and is generally done either infrequently or with great caution.

Some additional pages and examples might be available in the #bootstrapping tag

## Locally Attached vs. Remote Storage

During bootstrapping, some decisions have to be made.  One of the primary is "where will I store the state of this bootstrapping operation?"

There are two good options: Remote or Locally Attached Network storage

### Remote

Remote storage in the case of [[Terraform]] means using a remote backed provider, such as `S3`.

For example:

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.30"
    }
  }
  backend "s3" {
    bucket         = "my-state-bukkit"
    encrypt        = true
    key            = "bootstrap"
    profile        = "myprofile"
    region         = "us-east-1"
    dynamodb_table = "commtfstate"
  }

}

# Configure the AWS Provider
provider "aws" {
  profile = "myprofile"
  region  = "us-east-1"
}

```

With remote storage, one depends on the underlying provider to work properly.  It also means that one is depending on the existence of any preconditions and/or dependencies.

So for an `S3` back-end, the target bucket needs to exist.  How did that bucket get created?  Does it have appropriate IAM roles applied?  Needing to answer these questions doesn't mean that remote storage is a deal-breaker.  It just means that it might be necessary to have additional documentation ("HOWTO Create the TeamX Backend S3 Bucket").

### Locally Attached

This is _remote_ storage that is locally accessed as if it was _local_.  This might be a network share or some other type of local storage that gets synchronized to a location where anyone who has access to it can utilize it.

The most relevant version of this is the [[Keybase#KBFS|KBFS]] filesystem, which can be used to perform this task.  It still requires configuring a provider, but that provider works against the local filesystem (the KBFS root for a user or team).

For example:

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.30"
    }
  }
  backend "local" {
    path = "/keybase/team/mykeybaseteam/somesubdirectory/bootstrap-state/terraform.tfstate"
  }
}

# Configure the AWS Provider
provider "aws" {
  profile = "myprofile"
  region  = "us-east-1"
}

```

In this example, the KBFS filesystem is the target endpoint for the state file that [[Terraform]] is using.  Unfortunately, the `backed` setup within Terraform cannot use parameters or variables, so the `path =` must be set to conform to whatever root the KBFS takes.  On Linux, it is generally `/keybase` but on the Mac it is `/Volumes/kbfs`, and on Windows it defaults to `K:\`.  So the `path` above would need to be changed to reflect this..  This prefix needs to be handled prior to use to ensure that the local system is referencing the same shared state file in KBFS.
