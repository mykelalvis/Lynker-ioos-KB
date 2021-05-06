---
date updated: '2021-04-28T09:25:07-05:00'

---

# Version

<dl>
<dt>version</dt>
<dd>the entire identifying signature of an artifact, possibly but not necessarily one that has gone through a release process</dd>
</dl>

## Meta Definition
Wikipedia has [a brief article](https://en.wikipedia.org/wiki/Software_versioning) on the subject, but it does not entirely mesh with this definition.

A version of an artifact is the entire signature that formally identifies it.  Depending on the type of artifact, this might be as durable as the signature of a  compiled [[intermediate form]] stored in an [[artifact repository]], or it might be as transient as the URL of a [[tag release]] applied to a repository in [[GitHub]].

Versions can are divided into mutable and immutable types.  Most software does not contain sufficient complexity to distinguish between the two explicitly, but this is rarely cause for concern (until it is).

By convention, _mutable_ versions are considered transient and disposable.  That is, they have a short and indeterminate lifespan and, since they technically should be easy to replicate, can be thrown away at any time.  This conforms to the `-SNAPSHOT` concept present in [Maven](https.

In contrast, immutable versions should be retained in perpetuity.  They are never deleted or changed.  The expectation is that they will be available effectively forever.

In theory, the artifact that an immutable version references under those _conventional_ circumstances would itself be considered _immutable_.

In practice, this is one of the Hard Problems™, can be difficult to verify, and frequently requires a quantity of trust.

## Underlying Definition
Since a given tree of artifacts can be associated with some collection of namespace and artifact identifier, the "version" of a given artifact is usually thought of as simply the "version number".  This is commonly a [Semantic Version](https://semver.org), popularized as a formal version numbering system starting in 2009 and becoming increasingly more prescriptive across time. 