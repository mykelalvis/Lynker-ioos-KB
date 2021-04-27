---
date updated: '2021-03-08T09:06:57-06:00'

---

# Upstream Setup

If you want to use this vault as a [[Git]] upstream for injecting changes to your own vault, follow along:

1. Create a fork of the source repository.  This is likely [here](https://github.com/infrastructurebuilder/release-engineering)
2. Clone your fork locally
3. Change directory into your local clone.
4. `git remote add upstream git@github.com:infrastructurebuilder/release-engineering.git`
5. It is suggested to use the `master` branch as the default branch for your repo.
6. To pull changes from upstream:
7. `git checkout master`
8. `git rebase upstream/master`
9. Resolve conflicts
10. `git push` to your local copy.

This repo (`release-engineering`) _**might**_ accept a very limited PR from your downstream clone.

See [[Formatting]] for data about how these are...formatted.
