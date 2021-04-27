---
date updated: '2021-03-16T03:55:58-05:00'

---

# Infrastructure As Code

Infrastructure as Code (IAC) has now come into wide adoption.  IAC is an aspect of [[operations]] that forces the management of the [[idempotence#Operational State|operational state]] of an infrastructure to be dealt with as an aspect of [[configuration management]].

In an IAC environment, rather than placing the onus of operating the infrastructure onto the eyes, hands, and keyboards, of operations personnel, the infrastructure is configured using one or more IAC tools, such as [[Terraform]].  This allows a number of improvements over the process

1. The code can be stored in a [[source code]] repository (like [[git]]).
2. The code in that repository can be peer-reviewed.
3. The state of the system can be described without having to actually go look.
4. If a state starts to drift out of configuration, another [[idempotence|idempotent]] application of the IAC code generally brings it back into line.

IAC also comes with some costs.  Most of these costs are measured by additional time required to produce the IAC source code, and the increased complexity of understanding necessary to craft the correct IAC sources.
