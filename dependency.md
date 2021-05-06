# Dependencies

<dl>
<dt>dependency</dt>
<dd>An artifact or environment that another artifact or environment needs in order to proceed or exist.</dd>
</dl>

A dependency is a thing that [something else needs](https://seuss.fandom.com/wiki/Thneed#:~:text=A%20Thneed%20is%20a%20highly,to%20The%20Lorax%20(film).  For processes, it is a requirement for proceeding.  For an environment, it is something that must exist (and probably be accessible) in order for the target environment to exist.

## Resolution 
The act of gathering or creating dependencies of a given target is called dependency resolution.  It is performed using some implementation of a [Satisfiability Solver](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem) for the needed graph of dependencies.  For practical purposes, the graph of dependencies (for a given target) must be a directed acyclc graph.  Any other impl