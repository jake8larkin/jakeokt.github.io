---
layout: post
title: Configuring Ctrl-P in Vi
---


[CtrlP.vim](https://github.com/ctrlpvim/ctrlp.vim) is a fuzzy path, file finder plugin for the Vim editor.
I recently spent some time digging into the configuration options for Ctrl-P to make it perform better on my work systems.



```ruby
" ctrlp vim plug in override options
" use find for building file index instead of vim globbing (buggy in OSX)
let g:ctrlp_user_command = 'find %s -type f'
" increase the default max size of results
let g:ctrlp_match_window = 'max:35'
" enable cross session caching
let g:ctrlp_clear_cache_on_exit = 0
" max number fo files to scan
let g:ctrlp_max_files = 100000
let g:ctrlp_max_depth = 100
" default mode to open CtrlP in
let g:ctrlp_cmd = 'CtrlPMixed'

```
