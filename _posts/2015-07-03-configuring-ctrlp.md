---
layout: post
title: Configuring Ctrl-P in Vi
---

[CtrlP.vim](https://github.com/ctrlpvim/ctrlp.vim) is a fuzzy path, file finder plugin for the Vim editor.
I recently spent some time digging into the configuration options for Ctrl-P to make it perform better in my development environment. I switch between a fast Ubuntu desktop and slow Macbook Air. Some of this is driven as a attempt to iron out quirks and performance issues on the Mac. All configuration is entered into a `~/.vimrc`



By default CtrlP uses Vim's file path globbing to identify files to index. This can be a little buggy and slow in a large directory tree.  We can modify the config to use a different routine to find files. Here it's using shell `find` with some additional options to exclude paths matching certain patterns.
  -  exclude paths with target/   usually compilation artifacts in java or scala
  -  exclude the .git directory

```
let g:ctrlp_user_command = 'find %s -type f -not -path "*target/*" -not -path "*\.git*"'
```
By default only 10 matching results are shown. We can increase this to a larger amount depending on window size

```
let g:ctrlp_match_window = 'max:35'
```

CtrlP usually scans the current directory on the first time you open the pane in a fresh vim session.  Vim is single threaded so this creates a noticeable pause when starting Ctrl-P for the first time in a large project directory. Here I turn on cross-session caching, allowing CtrlP to save/load its index to/from a cache file. This enables faster startup, at the cost of the index sometimes getting stale. Hitting `<F5>` while the CtrlP pane is open forces a rebuild of the index.

```
let g:ctrlp_clear_cache_on_exit = 0
```

Let's bump up some low defaults for scan maximums

```
" max number fo files to scan
let g:ctrlp_max_files = 100000
let g:ctrlp_max_depth = 100
```

CtrlP can operate in 3 distinct modes, or a some combination of them. File, Buffer, and MRU.  File mode builds an search index on the directory tree starting from the current working directory.  Buffer, I haven't used much but assume, it only indexes the files that are currently open in Vim buffers.  MRU (Most Recently Used) includes recently opened files.   Here I enable Mixed mode which is a superset of all three individual modes.  I do find that MRU mode encourages some confusion if you're constantly switching between projects that have many identically named files. An example is searching for something generic like `Build.scala` or `Gemfile`. The top result with MRU mode may be a file in another directory/project.

```
let g:ctrlp_cmd = 'CtrlPMixed'
```

