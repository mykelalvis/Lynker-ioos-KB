# Compiled Executable Data Package Specification

 A [[Compiled Executable Data Package]] is simply an archive that conforms to this specification, its associated sha512 checksum file , and signature files for both of those file (total of 4 files).  These four files may themselves be distributed as an archive or as 4 independent files.

The archive may be of any of the following formats:
- `.zip` - InfoZip
- `.tgz` - Z-compressed tarball
- `.tar` - Uncompress tarball

The preferred methods are, in order,  `tgz`, `tar` and  `.zip`

The standards of the specification are as follows:
- Required presence of a valid [[Compiled Executable Manifest]]
	- Note that the manifest indicates the method for actually launching the compiled executable
- Required presence of compiled executables specified by the manifest.
	- Runtime linked executables
	- shared libraries
	- statically linked binaries
- Optional presence of the sources that produced this compiled code
