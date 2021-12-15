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

## Cloud

[Terraform Cloud](https://app.terraform.io/) is a Terraform [[#Registry]].