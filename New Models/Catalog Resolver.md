# Catalog Resolver

A Catalog Resolver is a versioned executable that takes a versioned catalog file and "resolves" the contents.  This means that all imports have been (recursively) acquired and that all defaults/imports/prototyped configurations have been applied.  

## Resolution Aspects

 A catalog consists of a YAML file conforming to whatever version of the Catalog Resolver supports.  All Catalog Resolver instances are tied directly to the version of the catalog that they support, and are not expected to support other versions of the catalog.  Resolvers are present for every valid published version of the Catalog model.  If no resolver is available, then that version of the catalog has no resolver because it is not valid.

### Deterministic
A catalog is expected to be produce deterministic outcomes unless its dependencies change.   Note that once a catalog has been resolved, re-resolution replaces the existing artifact.

### Unidirectional
Resolving a catalog is, effectively, a compilation process.  It produces an artifact that can be sent to a [[Catalog Executor]] , but does not supply information backwards.  That said, the resolver produces a resolved file as an interim artifact.  This artifact is also a catalog, so it can be exported and used as a catalog and re-fed to a Catalog Resolver.

### Cached
The catalog resolver store catalogs in a versioned catalog repository.  This repository is managed by a hash of the source file, so the resolver fetches any external imports, caches those, and hashes the supplied source file.  If the hashes of all the imports exists, and if the existing catalog hash already exists, then the catalog has not changed and is not refreshed.  The existing resolved cached version is used.

If the catalog is generated as a released version, then it is permanently cached and that version cannot be regenerated.

If the catalog is a snapshot, then the catalog resolver can be forced to regenerate with a flag.

