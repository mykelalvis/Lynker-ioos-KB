---
date updated: 2021-12-05 02:28
---

Tags: #fact

# Keybase

`URL: ` https://keybase.io

Keybase is a key sharing service that allows secure team interactions, with key management, chat, and [[#KBFS|file sharing]].  Keybase interactions are encrypted end-to-end, although with keys managed by the service itself.

# KBFS

Keybase Filesystem is a (FUSE-based) filesystem that is verify on read/write.

As it verifies access each time it tries to read or write, being ejected from the team that owns that KBFS means losing access immediately.

This does NOT prevent copies from being made.  This means that compromises of the keys, Keybase, or the KBFS generally prompt a [[rekey#Full Rekey|full rekey]].
