# Compiled Executable Manifest

This is a manifest of the contents of a [[Compiled Executable Data Package]].  

Such a data package is an archive of files.  

The manifest is explictly a file called `META-INF/CEDPManifest.yaml`  (i.e. a YAML file called `CEDPManifest.yaml` stored in the `META-INF` folder at the root of the archive)

The manifest fully describes the contents of the data package.


## Validation
A manifest file is a yaml file named `META-INF/CEDPManifest.yaml`

A  manifest file is valid when it has the following attributes:
- is a file called `META-INF/CEDPManifest.yaml` at the root of the archive
	- All [[#Elements]] must be validated
- has a verifiable GPG signature of that YAML file, also present in `META-INF`
- identifies all code entries within the archive
- has verifiable GPG signatures of all the code entries within the archive
- identifies optional source archives within the archive
- has verifiable GPG signatures of source archives
- specifies the method by which the code entries are to be executed

There (will) exist a program that will validate a given manifest by extracting the entries and validating them according to file type and signature

> [!info] Completeness
> All code entries must be documented in the manifest, and each entry (including sources) must have a GPG signature file with a `.asc` extension associated with it.  Any archive with files that are not in the manifest is considered non-conformant and invalid.


The elements of the manifest file located at `META-INF/CEDPManifest.yaml` are:
- [[#Identity]]
- [[#Documentation]]
- 

## Identity

### GroupId
All packages have a _namespace_.  This namespace must be verifiable as one that "belongs" to the creator of the [[Compiled Executable Data Package|CEDP]].

Note that a given groupId is _assigned_ when a new user is added to the system, based on a reversed-DNS named tree from their email.  Additional groupIds may be assignedm as well.

```
groupId: edu.someschool.somedept.someperson
```
### ArtifactId
Each package must have an artifact identifier.  This is a string that must conform to the regex`^[\w-_]+$`, and should be at least moderately descriptive.  This is usually the name of the source code repository if the source is from Git.  

```
artifactId: my-artifact
```
### Version
Each package must have a semantic version applied to it.  A package is uniquely identified by it's type (`cedp:`) and a combination of `GroupId:ArtifactId:Version`

The version is processed as a Maven version.  Thus, if the version ends in `-SNAPSHOT` it is considered an unreleased artifact.  Unreleased artifacts can be continually rebuilt and stored.  However, changing the version of the pacakge to a non-`-SNAPSHOT` version indicates release.  Once a specific version of an artifact is released, that specific version cannot be released again.  In order to perform another release, its version must be increased. 

```
version: 1.0.0
```
or 
```
version: 2.3.4-SNAPSHOT
```

> [!note] Length
> GroupId, ArtifactId, and Version are all limited to 240 characters each

> [!faq]  Quit asking this
> Don't do it!  ^thisisacalloutforafaq



## Documentation
### Name
All packages have a name.  That name does not have to be unique.  It is simply descriptive.  Names are arbitary strings, but are limited to 240 characters.  They may not contain markup.  The regex match for them is `[(\w\d_)+]`

### Description
Any package may have a textual description of the contents.  Most Markdown is acceptable, but currently is not rendered as such.  Description length is limited to 16,383 total characters, including whitespace.

> [!note] Formatting
> In the YAML of the manifest file, the description should be written as a multi-line string using `|-`



## Sources
A list of files may optionally be included as source files.  These files should themselves be archives, but need have no specific manifest internally.  They're just archives.

As a general rule, they will only take up space and rarely be opened.  However, the possibility is present so that forensics may be applied to a given compiled executable

```
source: 
- path/to/source/file1.zip
- path/to/source/file2.tgz
```

Sources are not expanded or verified, and need no checksums
## Code

Code entries are listed as an array of object with the following attributes:
```
path: # String path to the file within the archive
type: # output of the unix `file` command on the file
sha512: # the sha512 computation of this file (The checksum associated with `sha512sum ${path}` )
```

Any path that starts with `lib/` is expected to be a library.  That is the runtime's indication that the library should be included into the library path at execution time for dynamic linking.  Note that we will be expanding the directory according to the entries in the manifest, so anything not in the manifest _will not exist on the final running system_.


```
code:
- path: path/to/executable1
  type: "POSIX shell script, ASCII text executable"
  sha512: c638d5740171204621d9ea443e912fed9309e02745d9a979ec72abcd268b853a05320a328c26f290db39b4706650665821917ed004f96742d83f7d3b305f8151
- path: path/to/executable2
  type: "POSIX shell script, ASCII text executable"
  sha512: 060efb94d5ca1995a242908120cbeec13f24e207be5771050eff595b4f0fd128ab9b8e639d7f5064667b96e28eb822f8cddd21e8f8264e9ed73fff6bc076e4d5
- path: path/to/executable3.sh
  type: "POSIX shell script, ASCII text executable"
  sha512: b79683862fe8b2b6e0ed7add3eafd67cc1f0825c2941d42639667f3e4e701a9470cdfea99e79bf9e492afe6f393208e7e1aaf0fecc104962c68f2013b4264b1f
- path: lib/to/libary1
  type: "ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, stripped"
  sha512: 71144032f9bbfc27f3853896bd6b14cd2d6b4ddef593c7928a800fa19775b3262af3129f27bdf5a4705bb8f6ba71934fc27802f2441defcb1badd50c4102cfb1
- path: lib/to/libary2
  type: "ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, stripped"
  sha512: cf83e1357eefb8bdf1542850d66d8007d620e4050b5715dc83f4a921d36ce9ce47d0d13c5d85f2b0ff8318d2877eec2f63b931bd47417a81a538327af927da3e
- path: lib/to/libary3
  type: "ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, stripped"
  sha512: 5092c689e34d6575d8f28f642224fb7e4bd23c0f329cee8d10970ace9f1c6ea7418ba0885ccb2fdedaa36950a5a8f2c63c64656a230f695750c8b1f67fc8b242
```



## Example

#TODO

```
groupId: my.group.id
artifactId: thismodel
version: 1.0.0
name: This Model
description: |-
  Here is a multi-line
  description of thismodel
code:
- path: path/to/a/shell/script.sh
  type: "POSIX shell script, ASCII text executable"
  sha512: "30afe538561a53469276c3e93d03401f9d5a9d9c37643a62f442e1e7f3ad826dd65ec8ba553273031dbfd789ba1c89d7c825fca8eaf572cef7c25a9216287096"
- path: otherpath/to/a/python/script.py
  type: "Python script, ASCII text executable"
  sha512: "643a62f442e1e7f3ad826dd65ec8ba553273031dbfd789ba1c89d7c825fca8eaf572cef7c25a921628709630afe538561a53469276c3e93d03401f9d5a9d9c37"
- path: lib/path.so
  type: "ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=f8bb6ff5358896718dea2239b82775cbf47c93db, stripped"
  sha512: "e93d03401f9d5a9d9c37643a62f442e1e7f3ad826dd65ec8ba553273031dbfd789ba1c89d7c825fca8eaf572cef7c25a921628709630afe538561a53469276c3"
source
```


