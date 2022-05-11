---
date updated: '2021-10-08T08:23:17-05:00'
aliases:
  - parameterized

---

# Parameterized

Parameterization is the mechanism of extracting specific parts of configuration from some executive process and providing a means for supplying those parts differently according to different circumstances.

This is most notable in software development, where a nominal "function" might be defined as a "process that returns some result based on it's parameters".

This same effect applies to [[release engineering]], [[build management]], and the [[deployment]] process.  When properly parameterized, a specific build or deployment could perform very differently, based on those parameters.

## Predictability
Note that parameterization appears to fly in the face of a desire for deterministic outcomes.  This is not entirely true, though.  The key is "proper" parameterization.  

The result should remain deterministically predictable.  A failing build with no parameter changes should continue to fail;  a succeeding one should continue to succeed.

### Implicit Parameters
Implicit parameters are those that are inferred rather than provided.  For instance, when performing a [[build]] on a machine of a certain OS, it might be necessary to perform additional or fewer tasks to produce the same result.

### Explicit Parameters
Contrary to the implict parameters, explicit parameters are directly supplied to the process.  Most often in [[deployment]] this might be seen in the [12-factor app](https://12factor.net/), where environmental variables are the general means for configuration.  Tools like [[deployment#Standard Deployment Conditions|direnv]] can assist with this, as they allow a specific and manageable mechanism for establishing the explicit parameters.