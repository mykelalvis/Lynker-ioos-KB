---
date updated: '2021-03-16T03:56:07-05:00'

---

# Intermediate Form Releases

Tags: #release-engineering, #dependencies

Intermediate form builds are those that produce an [[artifact]] in some intermediate form, generally out of a [[source code]] repository.  This process is usually the result of a formal [[build]].  If this is the case, every effort should be made to ensure that it remains the case.

Most intermediate forms are best stored in some type of [[artifact repository]].  This allows the intermediate form to be used from a trusted location.

## Why Intermediate Forms?

The intermediate form of an [[artifact]] is one that has had a [[build]] (and possibly a [[release]]) action applied to it.  Therefore, it has gone through all the build [[gatekeeping]] operations and certain criteria can be said about the intermediate form without forcing a repeat of the [[build]] that produced it -- it was produced by some build, probably detectable through a tag, and thus followed the criteria of _that tag's build_.

These gatekeeping operations are the total of ascribable quality that can be stated about the artifact.  Assuming that they are [[build#Repeatable|repeatable]], they can be thought of as the beginning of an audit trail on the artifact's provenance and quality.

## Examples

Many, possibly even most, languages produce some artifact as an intermediate form.

- Java produces the `.jar` (Java Archive) artifact, which is a Zip file with some additional metadata files inside (`META-INF/MANIFEST.MF`).  Similar is the `.war` file, which is really just a `.jar` with a `WEB-INF/web.xml` tree inside.
- Similar but somewhat different is a Java `ear` (enterprise archive), which often contains [[dependency management|internal dependencies]]
- [NPM](https://www.npmjs.com/) packages
- Platform-specific executable statically linked binaries produce by `golang` or `rust`
- [RubyGems](https://rubygems.org/)
- Shared libraries (`.so` and `.o`) files -- this is a Bad Idea
- `.rpm` and `.deb` packages
- [PyPi](https://pypi.org/) packages (`.egg`, etc)
- `.zip` files of "stuff" produced from a build

Numerous others also exist.  Most languages use some intermediate form unless they exclusively use [[tag release|tag releases]], which are themselves a [somewhat weaker] intermediate form.
