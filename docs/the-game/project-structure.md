---
sidebar_position: 1
---

# Project structure

The project is divided into several parts:

- ForgeHX-{code}.js - the main game file
- merged-3rd-party-{code}.js - the file with the third-party libraries
- merged-game-{code}.js - the file with the game logic

In the file names there is a {code} placeholder, which is replaced with the cache-busting hash.

These files are minified, this make them hard to read.
