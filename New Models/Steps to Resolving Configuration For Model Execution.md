A "model run" is an [[Execution]] of a model configuration.

This configuration is in one (or more) files that are read into a [[Catalog Resolver]] to produce a [[Resolved Catalog]].  

A [[Resolved Catalog]] is then "realized" by means of a [[Catalog Realizer]], producing a [[Realized Catalog]] (which is a candidate for [[Execution]])

A [[Realized Catalog]] is passed to a (versioned) [[Catalog Executor]] to perform an [[Execution]] of the run defined within the [[Realized Catalog]].  

An [[Execution]] is a (potentially long-running) process that either succeeds or fails, producing an [[Execution Result]].
- Success/Fail preliminary results are performed immediately via configured notification channels.
- Successful and Failed execution results are effectively identical except exit code and possible branching of post-execution processing.  
- Named outputs are attached to the [[Execution Result]] as identifiers with URI endpoints pointed to files.  
- Dispositions are performed per the result disposition identifiers attached to the outbound result data.
- Final notifications are performed per configured notification channels.

