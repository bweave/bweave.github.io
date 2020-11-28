---
layout: post
title: "Vim/Nvim: Open multiple files in splits"
date: "2020-11-28 08:00:00 -0500"
tags: TIL Vim Nvim
---

Open multiple files in Vim/Nvim in splits with the `-o` (top/bottom) and `-O` (side-by-side) CLI flags.

```bash
vim -o foo.rb foo_test.rb
```

```bash
nvim -O bar.rb bar_test.rb
```

Wanna get fancy? Here's a handy way to work on [Exercism.io](https://exercism.io) Ruby exercises in Neovim using the built in terminal emulator along with the [rerun](https://github.com/alexch/rerun) gem to watch for changes and, you guessed it, rerun your tests.

```zsh
nvim -O *.rb term://"rerun -x --dir . --pattern '*.rb' -- ruby -r minitest/pride *_test.rb"
```

Take it to the next level with aliases for multiple Exercism tracks. Add this to your `.bashrc` or `.zshrc` to work with Ruby and Python:

```bash
alias exrb='nvim -O *.rb term://"rerun -x -b --dir . --pattern \"*.rb\" -- ruby -r minitest/pride *_test.rb"'
alias expy='nvim -O *.py term://"rerun -x -b --dir . --pattern \"*.py\" -- pytest *_test.py"'
```
