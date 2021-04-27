---
date updated: '2021-04-21T12:10:49-05:00'

---

# Platform

<dl>
<dt>platform</dt>
<dd>somewhere that you want some artifact to be available to its consumer</dd>
</dl>

A platform is a named target.  It is expected to be a place where you might induce [[availability]] of services by way of [[deployment|deploying]] [[artifact|artifacts]].

Specifically, the term "platform" is an abstraction.  For specific implementations, the term is "[[environment]]" (i.e. an "environment" is a "platform")

The runtime for an application could be considered the platform.  In addition, the platform that this platform runs on is _probably also_ an environment (and this a platform).

For example, [Artifactory](https://jfrog.com/artifactory/) is an [[artifact repository]].  As a tool, it is a "platform".  However, once deployed and given an endpoint that can be used to store and retrieve artifacts, it would be an [[environment]].
