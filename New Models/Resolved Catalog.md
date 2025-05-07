# Resolved Catalog

A Resolved Catalog was processed by a [[Catalog Resolver]] to produce a single, pre-runtime resolved catalog file.  This has all  information that is available prior to execution.

A resolved catalog still has placeholders for items to be runtime-resolved.
- No unknown or unresolved parts except runtime values such as [[Execution Environment Variables]] and [[Data Location Indicators]]
- Generated identities are present, but are held by placeholders to allow regeneration after [[Execution Environment Variables]] have been injected
