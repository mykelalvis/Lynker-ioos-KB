---
date updated: '2021-03-16T03:55:44-05:00'

---

# Continuous Integration

<dl>
<dt>continuous integration</dt>
<dd>the act of determining if a given change to an artifacts repository/source passes a known set of gatekeeping operations and notifying consumers about that state.</dd>
</dl>

A continuous integration (CI) service, such as Jenkins or Travis-CI, is a tool that watches for changes in sources (i.e. repositories) and performs some specific task (usually a [[build]]) against that changeset.  The outcome of that task is almost always meant to be a binary decision (pass or fail) based on whatever [[gatekeeping]] operations were present in the task definition itself.

The specific task that is run is _nearly always_ the [[build]] [[lifecycle]] up to some point that can be easily replicated.  This point is _nearly always_ getting past the "not tested" state. When such a CI build passes for a given changeset of a repository, it is because the "compile and test" parts of its build lifecycle were successful.  This allows the worker iterating on that repository to pass some of the work of "watching the build" off to the CI server, with the confidence that they will receive a notification of some sort when the end-state has been achieved and determination of success or failure is known.

Often, CI servers will be hooked into messaging services such that the notifications of success or failure do not require the worker to leave their work and go look at a status page on the CI server.  They can simply receive a "Build X passed" or "Build X failed" style notification in Slack, Teams, by text message, or email.  This is especially important when the task takes a long time to complete, or might have some partial success.

Continuous integration has been the _de facto_ operating standard of software development for several years now.  The lack of CI work on a project is an indicator of a lack of certain types of transparency, which in turn indicates a lack of some maturity.  This sort of implicit obfuscation is not necessarily an indication of poor software development practices, but it is generally perceived to be such.
