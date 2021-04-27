---
date updated: '2021-04-21T13:44:47-05:00'

---

# Packaging

Packaging is the process of producing a usable [[artifact]].  

## Types
A package can come in several forms, but the most common is some [[intermediate form]] (like a `jar` or `exe` or a `zip` file) or a [[tag release]].

Packaging is the final part of a standard [[build]] process.  For many iterations of the build-test process, packaging is not exercised.  This is especially true for [[tag release]] artifacts.  
## Metadata
Packaging, as a step in the [[build]] and [[release]] [[lifecycle]] might also have several other tasks assigned to it, such as building documentation or producing a [[cryptographic signature]].

## End State
Upon completion of the packaging phase, the actual artifact and any associated metadata are produced.  Once a packaging process reaches this target state, all the relevant [[artifact|artifacts]] have been produced and can be used or placed in the appropriate location (like an [[artifact repository]]).