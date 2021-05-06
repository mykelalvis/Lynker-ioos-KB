---
date updated: '2021-05-06T13:58:41-05:00'

---

# Dependencies

<dl>
<dt>dependency</dt>
<dd>An artifact or environment that another artifact or environment needs in order to proceed or exist.</dd>
</dl>

A dependency is a thing that [something else needs](https://seuss.fandom.com/wiki/Thneed).  For processes, it is a requirement for proceeding.  For an environment, it is something that must exist (and probably be accessible) in order for the target environment to exist.

## Resolution

The act of gathering or creating dependencies of a given target is called dependency resolution.  It is performed using some implementation of a [Satisfiability Solver](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem) for the needed graph of dependencies.

For practical purposes, it is usually most convenient for the graph of dependencies (for a given target) to be a directed acyclic graph.  Any other implementation is an n-step satisfaction execution, resulting in multiple possibile instances of success and failure.  Avoid this due to the difficulties of resolution.  Also, avoid it because it is easy to avoid.

In software, the resolution of dependencies is most often accomplished using the [[version]] of a the target's named dependencies.  A target process will define its dependencies as some list of identified artifacts, usually directly associated with a named [[version]]-based satisfaction condition of the target artifact.  Some dependency resolution action will take place based on the outcome of parsing the SAT solver being used, the resolution actions executed, and either the dependencies will be entirely resolved or the resolution process failed.

The good thing about dependency resolution, especially in the case of a directed acyclic graph, is that the resolution outcome is almost always that binary.  This allows us to look at the dependencies required for a target, attempt resolution, and to abort any following processes if the resolution does not fully succeed.

## Management

On average, somewhere between 65% and 95% of almost all software is composed of code that other people wrote.  The practice of use and reuse of software artifacts over time has increased, and is likely to continue to grow.  One of these "other people's property" is usually referred to as a [[dependency]].

Dependency management is the area of [[build management]] that focuses on being accurate and precise about how the dependencies of a given project are dealt with.
#TODO
