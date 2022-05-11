---
date updated: '2021-03-16T03:56:40-05:00'

---

# Terraform

[Terraform](https://terraform.io) is an [[IAC]] product of HashiCorp.  Terraform itself is only responsible for parsing the Hashicorp Configuration Language (HCL) supplied. All the underlying work is performed by one or more "providers". In Terraform languages, a [[#Providers|provider]] interacts with some [generally remote] service to perform ostensibly [[idempotence|idempotent]] changes to that service.

## HCL
The [Hashicorp Configuration Language](https://github.com/hashicorp/hcl) is a text-based source code suitable for commit to a [[source code]] repository.  It has limitations, but generally gets the job done.

## Providers

 A [Terraform Provider](https://www.terraform.io/docs/language/providers/index.html) is an underlying executable (sort of a "plugin") to Terraform that actually performs the interactions with the target provisioned service or element.

## Modules
Work in [[#HCL]] can be subdivided into [modules](https://www.terraform.io/docs/language/modules/develop/index.html) for improved scope management.

## Registry
A "registry" is a set of available [[#Providers]] and [[#Modules]] that the Terraform executable can access directly.  At present, the only known registry is [[#Cloud|Terraform Cloud]].

## State

Terraform has the concent of a "state" file, which is simply what Terraform thinks some set of resources should look like.  As long as the state and reality are identical, a `terraform plan` should be [[idempotence|idempotent]].  However, when Terraform's state differs from the target's state, commonly referred to as "drift", the plan would be for Terraform to apply changes to that target to make it conform to _what the local set of [[#HCL]] specifies_.  

## Backends
Terraform state will be storred in a local file by default.  However, different [[#Providers]] may allow [different storage mechanisms](https://www.terraform.io/docs/language/settings/backends/index.html) for the state. Most of these are remote, allowing other users to utilize and plan using that remote state.  Terraform [[#Cloud]] is the only officially supported backend, but many others exist.


## Cloud

[Terraform Cloud](https://app.terraform.io/) is a Terraform [[#Registry]].