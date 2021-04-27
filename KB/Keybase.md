---
date updated: '2021-03-16T03:57:10-05:00'

---
Tags: #fact 

# Keybase

Keybase is a key sharing service that allows team interactions.

# KBFS

Keybase Filesystem is a (FUSE-based) filesystem that is verify on read/write.

As it verifies access each time it tries to read or write, being ejected from the team that owns that KBFS means losing access immediately.

This does NOT prevent copies from being made, which means that compromises of the keys, Keybase, or the KBFS generally prompts a [[rekey#Full Rekey|full rekey]].
