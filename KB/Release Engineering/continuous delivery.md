---
date updated: '2021-03-16T03:55:38-05:00'

---

# Continuous Delivery

<dl>
<dt>continuous delivery</dt>
<dd>the act of performing a deployment based on the results of a build and release lifecycle execution.</dd>
</dl>

Continuous delivery (CD) is an extremely contentious topic.\
On the one hand, the idea that software could simply be deployed when its quality is of a certain level is intriguing.\
On the other hand, it is generally regarded that software is hard, all software has bugs, and some of those bugs might be extremely dangerous.  Therefore, maybe sending code directly to the "available" state is something that should be done with great intention.

CD comes with an inherent risk.  The mitigation of this risk has a lot to do with how much risk an enterprise is willing to bear, and how much work the enterprise is willing to put into the mitigation process itself.

As a general rule, most CD is not done into production. Rather, CD is usually done into development, testing, or staging environments. It is then observed for some period of time to determine that it has not become sentient and is plotting to destroy the world. Once the observation period has passed, a determination is made that the deployment should or should not be moved to production.

This determination is _usually_ informed by the aforementioned risk-reduction testing.  However, in many real-world scenarios one or more ~3 lb. neural net wetware processors are utilized to make that determination using this information.
