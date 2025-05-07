---
date created: 2024-09-24 11:10
aliases:
  - CEM
---

# Compiled Executable Model

> ![note]
> This refers to a DATA model, not a catalog.

This is a representation of an executable model.

A compiled executable model is one of several types of configurations that fully specifies which executable will be executed as part of an [[Execution]] and how to acquire that executable.

Note that all executables currently target Linux systems.  We do not support models running under Windows.

# Executable Delivery Types

There are, obviously, many ways to make this happen.

## Built From Source

`type: model-builder`
Probably the most reliable method is to have the system build the model from source.  This implies quite a lot of dependency management and configuration, but the resulting configuration is typically a very good general representation of the elements needed to build a given model executable.

The [[Model Builder]] is a completely separate process that produces implicitly versioned models to be delivered to target runtimes.  The configuration must supply a reference to a (separate) [[Builder Specification]], which is executed within a different workflow but produces an identified executable

It should be noted that having an executable successfully [[#built from source]] produces a [[Compiled Executable Data Package]].  Such a specification is then used to further deployment of the executable to target systems.

## As Packaged Data

`type: data-package`
Another method is to provide the executable as a [[Compiled Executable Data Package]], per the [[Compiled Executable Data Package Specification]].  This package would not be created, but would rather be pulled directly from specified storage locations and the archive expanded to local storage.  This type of delivery is likely to require a bit of compatibility work and is not recommended as an initial method for delivery.

## Container

`type:  container`
The simplest, but not the most reliable, method is to provide a reference to a container, per the [[Compiled Container Specifications]].  By default, pulls for containers would be from Docker Hub, but if the image was stored in another container registry, that information would need to be provided.

## Cloud-provider Image

`type: provider-image`
The next simplest method is to provide a compiled image, per the [[Compiled Image Specifications]].  For an image to be considered resolved, it would need to be available by whatever cloud provider identifier denoted it.
