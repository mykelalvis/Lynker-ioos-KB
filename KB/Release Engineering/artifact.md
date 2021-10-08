---
date updated: '2021-10-08T07:58:24-05:00'
aliases:
  - artifacts

---

# Artifact

## Definition

> something produced somehow; a metaphorical bucket of bytes in software terms

An artifact is something we care about.  For purposes of this document, it's a collection of bytes that composes some sort of semantic meaning within our company.  This could be a document, a collection of data, or a compiled and tested piece of code.

## Examples

A few examples of artifacts are:

- A `.jar` file from a Java Maven or Gradle build.
- A statically linked OS-specific binary from a `golang` build.
- A PDF from building a set of markdown files using `pandoc`
- A `.zip` file created from collecting a set of documents and using InfoZip to create a ZIP file from them.
- A `.docx` generated with Microsoft Word or LibreOffice.

The goal of a delivery process is to reach the state of a released artifact.

## Metadata

As artifacts are data, and metadata is generally the data about data, then the metadata for an artifact is the data about the artifact.

A [[release|released]] artifact has attributes beyond those of an unreleased artifact.

- Artifacts are generally [[version|versioned]].
- An unreleased artifact _probably_ has a [transient] version, but the resulting artifact is not fixed and therefore cannot be relied on without additional risk above that of a fixed version.
