---
date updated: '2021-04-28T14:23:39-05:00'
tags:
  - '#TODO'

---

# Identity

Some of the harder problems to manage within release engineering is determining who did something (authentication) and who is allowed to do something (authorization).  These hard problems exist within essentially all processes.  They are not unique to release engineering.

This is generally an aspect of [[security]].  As such, there is almost always a provider for identity within a given organization.  This is the "identity provider".  There may be multiple identity providers, in which case some may have different provenances or intentions.  If all the identity provider instances are aggregated into a single provision mechanism they are referred to as being "federated".

Note that federated identity management is often quite difficult and just having all the providers in one place is not sufficient to claim true federation.
