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

## Completeness

An artifact may or may not be self-contained.  

### Self-Contained
If an artifact is self-contained, then nothing beyond the artifact itself is required other than the underlying [[environment]] is required to perform whatever task the artifact performs.

Examples of this are 
- statically linked binaries, such as produced by `golang` or `rust`
- So-called "uber-jars" for Java, which contain everything needed to execute
- A `.zip` file with a complete document or datafile 

Self-contained artifacts have benefits and drawbacks. The primary benefit is that one needs not acquire anything else to do whatever task is set forth for the artifact.  Many people find this a very compelling argument.   One drawback is that the size of the artifact itself is almost always necessarily larger than one that performs the same task but is not self-contained.  Another is that it can be more difficult to see what composes a self-contained artifact, as they are often intentionally obfuscated.

### Dependent
Many artifact, maybe even most of them, are not [[#Self-Contained]].  A dependent artifact is one that requires _other_ artifacts to perform its task.  This includes
- Most library artifacts that are themselves not meant to perfom an end-user action.  
	- This is true for most languages, like java or javascript
- Many final artifacts that simply rely on a [[dependency]] resolution mechanism to build the final executable at runtime.  
	- This sort of behavior is often very language-specific, such as a Maven run which performs a large [[dependency#Resolution|resolution action]] to acquire its executing classpath, or a Ruby application that executes with Bundler.  

While some people will claim that one way or the other is somehow technically superior, that is _very contextually dependent_ and frequently just a prelude to a [[holy war]].

