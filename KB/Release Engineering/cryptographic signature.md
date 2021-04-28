---
date updated: '2021-04-28T08:41:26-05:00'

---

# Cryptographic Signatures

A common practice during the [[build]] process, and especially during the [[packaging]] phase of a build that produces an [[intermediate form]], is to cryptographically sign the resultant artifact.  This allows for the verification of [[binary identity]] for a given [[artifact]] which provides verification that a given key was used to sign the resultant intermediate collection of bytes.

Note that for simple [[tag release|tag releases]], the use of a "signed commit" is very sufficient to denoted identity, assuming that the key used to produce the signed commit has published the public half.

Cryptographic signing is almost always done with an assymmetric key pair.  Furthermore, it is almost always done with [GPG](https://gnupg.org/).  The reasons for this are generally historical, but for the most part GPG serves this purpose admirably[^manual].

The result of a signature process is a collection of bytes (usually a text string) that contains a signature that can be verified against the original source file using a the public part of a key.  If the verification process works, then a given target file will successfully verify that the collection of bytes checked is the same collection of bytes that generated the signature.

[^manual]: A reasonably good explanation of this in [the manual](https://www.gnupg.org/gph/en/manual/x135.html)
