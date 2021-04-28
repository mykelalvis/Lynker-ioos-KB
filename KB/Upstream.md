---
date updated: '2021-03-16T03:57:31-05:00'

---

# Upstream Setup

If you want to use this vault as a [[git]] upstream for injecting changes to your own vault, follow along:

1. Create a fork of the source repository.  This is likely [here](https://github.com/infrastructurebuilder/release-engineering-vault)
2. Clone your fork locally
3. Change directory into your local clone.
4. `git remote add upstream git@github.com:infrastructurebuilder/release-engineering.git`
5. It is suggested to use the `master` branch as the default branch for your repo.
6. To pull changes from upstream:
7. `git checkout master`
8. `git rebase upstream/master`
9. Resolve conflicts
10. `git push` to your local copy.

`release-engineering` _**might**_ accept a very limited PR from your downstream clone.

See [[formatting]] for data about how these are...formatted.
