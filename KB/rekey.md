---
date updated: '2021-03-16T03:57:23-05:00'

---

# Rekey

# Intro

Rekeying is the act of dropping old keys and applying new ones.  It's identical to the process for rekeying a physical lock:  Generate a new key, change the system to use the new key, and throw away the old key.

# Reasons

A rekey is generally prompted when a person with some degree of access has had that access compromised.  There are numerous reasons why this might be the case:

1. Someone might have been terminated.  Now their access constitutes a liability.
2. Someone might have had a breach of the systems where the keys are stored.  Now the existing keys constitute a liability.
3. It might be time to recycle keys, which should be done on some scheduled basis.
4. It might be time to practice a rekeying effort, even though none needs to be done.

# Scopes

There are numerous levels of rekeying action.

## Specific Rekey

This is the regeneration of a single key or known subset of keys related to some context.  For instance, a teammate leaving the team prompts the necessity to remove and replace all keys that they had access to.

## Full Rekey

A full rekey is quite an undertaking.  It assumes that any and all secrets may have been compromised.  This frequently takes the form of having to drop all secret stores of any time and regenerating everything from scratch.

A full rekey should not be undertaken lightly, but it is essential when anyone who has full key access, such as an admin in [[DevSecOps]].

A full rekey should be executed at least annually to ensure that it is possible to perform one within an acceptable period of time.
