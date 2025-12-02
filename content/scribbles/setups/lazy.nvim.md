---
title: Lazy Vim Package Manager
draft: false
tags: ['vim','neovim','lua']
date: 2025-12-01
---

### What ?
+ Package Manager for neo vim.(reference [github](https://github.com/folke/lazy.nvim))

### Use ?

Bootstrap by creating a *lazy.lua* file under namespace and create a sample configuration and add dependency in the *init.lua* as *require ("<namespace>.lazy")*

```lua
local lazypath = vim.fn.stdpath "data" .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
    vim.fn.system {
        "git",
        "clone",
        "--filter=blob:none",
        "https://github.com/folke/lazy.nvim.git",
        "--branch=stable",
        lazypath
    }
end
vim.opt.rtp:prepend(lazypath) -- rtp - run time path

require("lazy").setup {
    spec = LAZY_PLUGIN_SPEC, -- global spec table
    install = {
        colorscheme = {"darkplus", "default"}
    },
    ui = {
        border = "rounded"
    },
    change_detection = {
        enabled = true,
        notify = false
    }
}

```

The following code appends the lazy module to the standard data path of vim  
```lua
local lazypath = vim.fn.stdpath "data" .. "/lazy/lazy.nvim"
```

In windows, `lua print(vim.fn.stdpath("data"))` will return ![image.png](../assets/image_1712002733167_0.png)  
To toggle Lazy.nvim window, use ==Lazy== command  
![image.png](../assets/image_1712003514001_0.png)

### Plugins ?

To use plugins using lazy, create a corresponding <plugin>.lua file under namespace and provide the configuration like this (colorscheme configuration):

```lua
local SPEC = {
    "LunarVim/darkplus.nvim",
    lazy = false, -- make sure we load this during startup if it is your main colorscheme
    priority = 1000 -- make sure to load this before all the other start plugins
}

function SPEC.config()
    vim.cmd.colorscheme "darkplus"
end

return SPEC

```
