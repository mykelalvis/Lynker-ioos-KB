---
date updated: '2021-04-28T13:56:15-05:00'
tags:
  - '#TODO'

aliases : [artifacts]

---

# Artifact

## Definition

<dl>
<dt>artifact</dt>
<dd>something produced somehow</dd>
<dd>a metaphorical bucket of bytes in software terms</dd>
</dl>

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

Artifacts also have metadata.  A [[release|released]] artifact has attributes beyond those of an unreleased artifact.

- Artifacts are generally [[version|versioned]].
- An unreleased artifact probably has a version, but the resulting artifact is not fixed and therefore cannot be relied on safely.

#TODO
