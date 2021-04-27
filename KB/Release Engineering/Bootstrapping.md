---
date updated: '2021-03-08T07:35:00-06:00'

---

# Bootstrapping

Bootstrapping, in this context, is the process of using automation to setup further automation.

For example, in order to have `S3` buckets for keeping [[Terraform]] state, you need several things -- not the least of which is a bucket to keep that state in.  

However, creating the bucket by hand _also_ has some problems.  

Thus, you might write some automation (which stores its state _locally_) which produces a bucket to store subsequent state _remotely_.  

The first automation is the "bootstrap", and is generally done either infrequently or with great caution.

Some additional pages and examples might be available in the #bootstrapping tag
