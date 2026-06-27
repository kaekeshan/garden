---
title: Neovim - Setup
description: This page walks through the Neovim configuration I use day-to-day on Windows. It assumes you already have lazy-nvim set up as the package manager — every plugin spec below is a .lua file under your...
draft: false
tags: [vim, tutorial]
date: 2025-11-30
---

This page walks through the Neovim configuration I use day-to-day on Windows. It assumes you already have [[lazy-nvim|Lazy.nvim]] set up as the package manager — every plugin spec below is a `*.lua` file under your namespace directory that `require("namespace.lazy")` will pick up.

## Folder layout

On Windows the config file is `init.lua` and it lives at:

```
C:\Users\<user>\AppData\Local\nvim\init.lua
```

Inside the same `nvim` directory, create a namespace folder (I use `kaekeshan`) and split the config into separate files:

![image.png](../assets/image_1711998665199_0.png)

The three files you need first:

| File | Purpose |
|------|---------|
| `launch.lua` | Defines `LAZY_PLUGIN_SPEC` (a global table) and a `spec(item)` helper to register plugins. |
| `options.lua` | Core `vim.opt` settings — clipboard, splits, indent, search behaviour, etc. |
| `keymaps.lua` | Global keymaps — leader key, window navigation, mouse-driven LSP, Tab/S-Tab behaviour. |

`init.lua` pulls them together:

```lua
require("namespace.launch")   -- registers plugins
require("namespace.options")
require("namespace.keymaps")
```

## launch.lua

A small helper that builds the global plugin table lazy.nvim consumes:

```lua
LAZY_PLUGIN_SPEC = {} -- global table variable

function spec(item)
    table.insert(LAZY_PLUGIN_SPEC, {import = item}) -- adds imports received as inputs
end
```

## options.lua

Core editor options. These are the values that have stuck over time — paste, tweak, restart:

```lua
vim.opt.backup = false           -- don't create a backup file
vim.opt.clipboard = "unnamedplus" -- share with system clipboard
vim.opt.cmdheight = 1             -- more space in the command line
vim.opt.completeopt = {"menuone", "noselect"} -- mostly for cmp
vim.opt.conceallevel = 0          -- so that `` is visible in markdown files
vim.opt.hlsearch = true           -- highlight all matches on previous search
vim.opt.ignorecase = true         -- ignore case in search patterns
vim.opt.mouse = "a"               -- allow the mouse everywhere
vim.opt.pumheight = 10            -- popup menu height
vim.opt.pumblend = 10
vim.opt.showmode = false          -- hide the "-- INSERT --" indicator
vim.opt.showtabline = 1           -- always show tabs
vim.opt.smartcase = true
vim.opt.smartindent = true
vim.opt.splitbelow = true         -- horizontal splits go below current window
vim.opt.splitright = true         -- vertical splits go to the right
vim.opt.swapfile = false
vim.opt.termguicolors = true      -- enable GUI colors in the terminal
vim.opt.timeoutlen = 1000         -- ms to wait for a mapped sequence
vim.opt.undofile = true           -- persistent undo
vim.opt.updatetime = 100          -- faster completion (default 4000ms)
vim.opt.writebackup = false

vim.opt.expandtab = true          -- tabs -> spaces
vim.opt.shiftwidth = 2
vim.opt.tabstop = 2
vim.opt.cursorline = true
vim.opt.number = true
vim.opt.laststatus = 3
vim.opt.showcmd = false
vim.opt.ruler = false
vim.opt.relativenumber = true
vim.opt.numberwidth = 4
vim.opt.signcolumn = "yes"        -- prevents text shifting when signs toggle
vim.opt.wrap = false
vim.opt.scrolloff = 0
vim.opt.sidescrolloff = 8
vim.opt.guifont = "monospace:h17"
vim.opt.title = false

vim.opt.fillchars = vim.opt.fillchars + "eob: "
vim.opt.fillchars:append {
    stl = " "
}

vim.opt.shortmess:append "c"

vim.cmd "set whichwrap+=<,>,[,],h,l"
vim.cmd [[set iskeyword+=-]]

vim.g.netrw_banner = 0
vim.g.netrw_mouse = 2
```

## keymaps.lua

Leader-key setup, window navigation, mouse-driven LSP popups, and visual-mode helpers:

```lua
local keymap = vim.keymap.set
local opts = {noremap = true, silent = true}

-- Leader key: Space
keymap("n", "<Space>", "", opts)
vim.g.mapleader = " "
vim.g.maplocalleader = " "

keymap("n", "<C-i>", "<C-i>", opts)

-- Better window navigation (Alt + hjkl)
keymap("n", "<m-h>", "<C-w>h", opts)
keymap("n", "<m-j>", "<C-w>j", opts)
keymap("n", "<m-k>", "<C-w>k", opts)
keymap("n", "<m-l>", "<C-w>l", opts)
keymap("n", "<m-tab>", "<c-6>", opts)

-- Stay in indent mode
keymap("v", "<", "<gv", opts)
keymap("v", ">", ">gv", opts)

-- Paste in visual mode without yanking
keymap("x", "p", [["_dP]])

-- Mouse menu (right-click and Tab)
vim.cmd [[:amenu 10.100 mousemenu.Goto\ Definition <cmd>lua vim.lsp.buf.definition()<CR>]]
vim.cmd [[:amenu 10.110 mousemenu.References <cmd>lua vim.lsp.buf.references()<CR>]]
vim.keymap.set("n", "<RightMouse>", "<cmd>:popup mousemenu<CR>")
vim.keymap.set("n", "<Tab>", "<cmd>:popup mousemenu<CR>")

-- Shift-h/l jump to first/last non-blank
keymap({"n", "o", "x"}, "<s-h>", "^", opts)
keymap({"n", "o", "x"}, "<s-l>", "g_", opts)

-- gj/gk for soft-wrapped lines
keymap({"n", "x"}, "j", "gj", opts)
keymap({"n", "x"}, "k", "gk", opts)

-- Toggle wrap
keymap("n", "<leader>w", ":lua vim.wo.wrap = not vim.wo.wrap<CR>", opts)

-- Exit terminal insert mode with Ctrl-;
vim.api.nvim_set_keymap("t", "<C-;>", "<C-\\><C-n>", opts)
```

## Plugins

Each subsection below is one plugin spec file. They all share the same `local SPEC = { ... }; function SPEC.config() ... end; return SPEC` shape.

### Dev-Icons

File-type icons in UI elements (completion menu, tree, statusline).

- Repository: [nvim-tree/nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons)

```lua
local devicons = {
    "nvim-tree/nvim-web-devicons",
    event = "VeryLazy"
}

function devicons.config()
    require "nvim-web-devicons"
end

return devicons
```

### Tree-sitter

Incremental parsing-based syntax highlighting and indentation.

- Repository: [nvim-treesitter/nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)

```lua
local SPEC = {
    "nvim-treesitter/nvim-treesitter",
    event = {"BufReadPost", "BufNewFile"},
    build = ":TSUpdate"
}

function SPEC.config()
    require "nvim-treesitter.install".compilers = {"zig"} -- install zig via system package manager
    require("nvim-treesitter.configs").setup {
        ensure_installed = {
            "lua", "markdown", "markdown_inline",
            "bash", "python", "c", "cpp", "rust",
            "java", "javascript", "html", "css", "csv"
        },
        highlight = {enable = true},
        indent = {enable = true}
    }
end

return SPEC
```

>[!tip] Manage parsers with `:TSUpdate`, `:TSInstall <lang>`, `:TSUninstall <lang>`.

### Hardtime

Discourages bad habits (e.g. repeated `h/j/k/l` instead of word motions) by briefly blocking the key.

- Repository: [m4xshen/hardtime.nvim](https://github.com/m4xshen/hardtime.nvim)

```lua
local SPEC = {
    "m4xshen/hardtime.nvim",
    dependencies = {"MunifTanjim/nui.nvim", "nvim-lua/plenary.nvim"},
    opts = {}
}

function SPEC.config()
    require("hardtime").setup()
end

return SPEC
```

### Mason + Mason-LSPConfig

Package manager for LSP servers, linters, formatters. The list below is the set of servers I auto-install:

- Repository: [williamboman/mason-lspconfig.nvim](https://github.com/williamboman/mason-lspconfig.nvim)

```lua
local SPEC = {
    "williamboman/mason-lspconfig.nvim",
    dependencies = {
        "williamboman/mason.nvim"
    }
}

function SPEC.config()
    local servers = {
        "clangd",
        "cssls",
        "jsonls",
        "quick_lint_js",
        "jedi_language_server",
        "rust_analyzer",
        "sqlls",
        "biome",
        "lemminx",
        "html",
        "lua_ls",
        "jdtls",
        "marksman"
    }

    require("mason").setup {
        ui = {border = "rounded"}
    }

    require("mason-lspconfig").setup {
        ensure_installed = servers
    }
end

return SPEC
```

### Schemastore

JSON schema catalog, used by `jsonls` to validate configs.

- Repository: [b0o/SchemaStore.nvim](https://github.com/b0o/SchemaStore.nvim)

```lua
local SPEC = {
    "b0o/schemastore.nvim",
    lazy = true
}

function SPEC.config()
end

return SPEC
```

### Lspconfig

The main LSP wiring — buffer keymaps, capabilities, per-server setups. Per-server settings live under `lua/user/lspsettings/<server>.lua`; see [[language-server-configs|Neovim - LSP]] for examples.

- Repository: [neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)

```lua
local SPEC = {
    "neovim/nvim-lspconfig",
    event = {"BufReadPre", "BufNewFile"},
    dependencies = {
        {
            "folke/neodev.nvim"
        }
    }
}

local function lsp_keymaps(bufnr)
    local opts = {noremap = true, silent = true}
    local keymap = vim.api.nvim_buf_set_keymap
    keymap(bufnr, "n", "gD", "<cmd>lua vim.lsp.buf.declaration()<CR>", opts)
    keymap(bufnr, "n", "gd", "<cmd>lua vim.lsp.buf.definition()<CR>", opts)
    keymap(bufnr, "n", "K",  "<cmd>lua vim.lsp.buf.hover()<CR>",       opts)
    keymap(bufnr, "n", "gI", "<cmd>lua vim.lsp.buf.implementation()<CR>", opts)
    keymap(bufnr, "n", "gr", "<cmd>lua vim.lsp.buf.references()<CR>",  opts)
    keymap(bufnr, "n", "gl", "<cmd>lua vim.diagnostic.open_float()<CR>", opts)
end

SPEC.on_attach = function(client, bufnr)
    lsp_keymaps(bufnr)

    if client.supports_method "textDocument/inlayHint" then
        vim.lsp.inlay_hint.enable(bufnr, true)
    end
end

function SPEC.common_capabilities()
    local capabilities = vim.lsp.protocol.make_client_capabilities()
    capabilities.textDocument.completion.completionItem.snippetSupport = true
    return capabilities
end

SPEC.toggle_inlay_hints = function()
    local bufnr = vim.api.nvim_get_current_buf()
    vim.lsp.inlay_hint.enable(bufnr, not vim.lsp.inlay_hint.is_enabled(bufnr))
end

function SPEC.config()
    local wk = require "which-key"
    wk.register {
        ["<leader>la"] = {"<cmd>lua vim.lsp.buf.code_action()<cr>", "Code Action"},
        ["<leader>lf"] = {
            "<cmd>lua vim.lsp.buf.format({async = true, filter = function(client) return client.name ~= 'typescript-tools' end})<cr>",
            "Format"
        },
        ["<leader>li"] = {"<cmd>LspInfo<cr>", "Info"},
        ["<leader>lj"] = {"<cmd>lua vim.diagnostic.goto_next()<cr>", "Next Diagnostic"},
        ["<leader>lh"] = {"<cmd>lua require('user.lspconfig').toggle_inlay_hints()<cr>", "Hints"},
        ["<leader>lk"] = {"<cmd>lua vim.diagnostic.goto_prev()<cr>", "Prev Diagnostic"},
        ["<leader>ll"] = {"<cmd>lua vim.lsp.codelens.run()<cr>", "CodeLens Action"},
        ["<leader>lq"] = {"<cmd>lua vim.diagnostic.setloclist()<cr>", "Quickfix"},
        ["<leader>lr"] = {"<cmd>lua vim.lsp.buf.rename()<cr>", "Rename"}
    }

    wk.register {
        ["<leader>la"] = {
            name = "LSP",
            a = {"<cmd>lua vim.lsp.buf.code_action()<cr>", "Code Action", mode = "v"}
        }
    }

    local lspconfig = require "lspconfig"
    local icons = require "kaekeshan.icons" -- extra dependency

    local servers = {
        "clangd", "cssls", "jsonls", "quick_lint_js",
        "jedi_language_server", "rust_analyzer", "sqlls",
        "biome", "lemminx", "html", "lua_ls", "jdtls", "marksman"
    }

    local default_diagnostic_config = {
        signs = {
            active = true,
            values = {
                {name = "DiagnosticSignError",  text = icons.diagnostics.Error},
                {name = "DiagnosticSignWarn",   text = icons.diagnostics.Warning},
                {name = "DiagnosticSignHint",   text = icons.diagnostics.Hint},
                {name = "DiagnosticSignInfo",   text = icons.diagnostics.Information}
            }
        },
        virtual_text = false,
        update_in_insert = false,
        underline = true,
        severity_sort = true,
        float = {
            focusable = true,
            style = "minimal",
            border = "rounded",
            source = "always",
            header = "",
            prefix = ""
        }
    }

    vim.diagnostic.config(default_diagnostic_config)

    for _, sign in ipairs(vim.tbl_get(vim.diagnostic.config(), "signs", "values") or {}) do
        vim.fn.sign_define(sign.name, {texthl = sign.name, text = sign.text, numhl = sign.name})
    end

    vim.lsp.handlers["textDocument/hover"]         = vim.lsp.with(vim.lsp.handlers.hover,         {border = "rounded"})
    vim.lsp.handlers["textDocument/signatureHelp"] = vim.lsp.with(vim.lsp.handlers.signature_help, {border = "rounded"})
    require("lspconfig.ui.windows").default_options.border = "rounded"

    for _, server in pairs(servers) do
        local opts = {
            on_attach = SPEC.on_attach,
            capabilities = SPEC.common_capabilities()
        }

        local require_ok, settings = pcall(require, "user.lspsettings." .. server)
        if require_ok then
            opts = vim.tbl_deep_extend("force", settings, opts)
        end

        if server == "lua_ls" then
            require("neodev").setup {}
        end

        lspconfig[server].setup(opts)
    end
end

return SPEC
```

>[!warning] Two required dependencies for `lspconfig`
>
> - [which-key](https://github.com/folke/which-key.nvim) for the `<leader>l*` mappings.
> - The icons function (`require "kaekeshan.icons"`) used by the diagnostic signs. See [Icons](#icons) below.

### Icons

The `icons` table used by `lspconfig`, `cmp`, and `devicons` to render glyphs for kinds, git statuses, diagnostics, and UI elements. This is a data file — keep it as is, or fork the icon mappings to your taste.

<details>
<summary>Show the icons catalog</summary>

```lua
return {
    kind = {
        Array = " ",
        Boolean = " ",
        Class = " ",
        Color = " ",
        Constant = " ",
        Constructor = " ",
        Enum = " ",
        EnumMember = " ",
        Event = " ",
        Field = " ",
        File = " ",
        Folder = " ",
        Function = " ",
        Interface = " ",
        Key = " ",
        Keyword = " ",
        Method = " ",
        Module = " ",
        Namespace = " ",
        Null = " ",
        Number = " ",
        Object = " ",
        Operator = " ",
        Package = " ",
        Property = " ",
        Reference = " ",
        Snippet = " ",
        String = " ",
        Struct = " ",
        Text = " ",
        TypeParameter = " ",
        Unit = " ",
        Value = " ",
        Variable = " "
    },
    git = {
        LineAdded = " ",
        LineModified = " ",
        LineRemoved = " ",
        FileDeleted = " ",
        FileIgnored = "◌",
        FileRenamed = " ",
        FileStaged = "S",
        FileUnmerged = " ",
        FileUnstaged = "",
        FileUntracked = "U",
        Diff = " ",
        Repo = " ",
        Octoface = " ",
        Copilot = " ",
        Branch = ""
    },
    ui = {
        ArrowCircleDown = "",
        ArrowCircleLeft = "",
        ArrowCircleRight = "",
        ArrowCircleUp = "",
        BoldArrowDown = "",
        BoldArrowLeft = "",
        BoldArrowRight = "",
        BoldArrowUp = "",
        BoldClose = "",
        BoldDividerLeft = "",
        BoldDividerRight = "",
        BoldLineLeft = "▎",
        BoldLineMiddle = "┃",
        BoldLineDashedMiddle = "┋",
        BookMark = " ",
        BoxChecked = " ",
        Bug = " ",
        Stacks = "",
        Scopes = "",
        Watches = "",
        DebugConsole = " ",
        Calendar = " ",
        Check = " ",
        ChevronRight = "",
        ChevronShortDown = "",
        ChevronShortLeft = "",
        ChevronShortRight = "",
        ChevronShortUp = "",
        Circle = " ",
        Close = "",
        CloudDownload = " ",
        Code = "",
        Comment = "",
        Dashboard = "",
        DividerLeft = "",
        DividerRight = "",
        DoubleChevronRight = "»",
        Ellipsis = "",
        EmptyFolder = " ",
        EmptyFolderOpen = " ",
        File = " ",
        FileSymlink = "",
        Files = " ",
        FindFile = "",
        FindText = "",
        Fire = " ",
        Folder = " ",
        FolderOpen = " ",
        FolderSymlink = " ",
        Forward = " ",
        Gear = " ",
        History = " ",
        Lightbulb = " ",
        LineLeft = "▏",
        LineMiddle = "│",
        List = " ",
        Lock = " ",
        NewFile = " ",
        Note = " ",
        Package = " ",
        Pencil = " ",
        Plus = " ",
        Project = " ",
        Search = " ",
        SignIn = " ",
        SignOut = " ",
        Tab = "",
        Table = " ",
        Target = " ",
        Telescope = " ",
        Text = " ",
        Tree = "",
        Triangle = "",
        TriangleShortArrowDown = "",
        TriangleShortArrowLeft = "",
        TriangleShortArrowRight = "",
        TriangleShortArrowUp = ""
    },
    diagnostics = {
        BoldError = "",
        Error = " ",
        BoldWarning = "",
        Warning = " ",
```

</details>

### Which-Key

Pops up a hint when you start a leader-key sequence and you pause. The `mappings` table below lists every `<leader>…` group I expose.

- Repository: [folke/which-key.nvim](https://github.com/folke/which-key.nvim)

```lua
local SPEC = {
    "folke/which-key.nvim"
}

function SPEC.config()
    local mappings = {
        q = {"<cmd>confirm q<CR>", "Quit"},
        h = {"<cmd>nohlsearch<CR>", "NOHL"},
        [";"] = {"<cmd>tabnew | terminal<CR>", "Term"},
        v = {"<cmd>vsplit<CR>", "Split"},
        b = {name = "Buffers"},
        d = {name = "Debug"},
        f = {name = "Find"},
        g = {name = "Git"},
        l = {name = "LSP"},
        p = {name = "Plugins"},
        t = {name = "Test"},
        a = {
            name = "Tab",
            n = {"<cmd>$tabnew<cr>", "New Empty Tab"},
            N = {"<cmd>tabnew %<cr>", "New Tab"},
            o = {"<cmd>tabonly<cr>", "Only"},
            h = {"<cmd>-tabmove<cr>", "Move Left"},
            l = {"<cmd>+tabmove<cr>", "Move Right"}
        },
        T = {name = "Treesitter"}
    }

    local which_key = require "which-key"
    which_key.setup {
        plugins = {
            marks = true,
            registers = true,
            spelling = {enabled = true, suggestions = 20},
            presets = {
                operators = false, motions = false, text_objects = false,
                windows = false, nav = false, z = false, g = false
            }
        },
        window = {border = "rounded", position = "bottom", padding = {2, 2, 2, 2}},
        ignore_missing = true,
        show_help = false,
        show_keys = false,
        disable = {buftypes = {}, filetypes = {"TelescopePrompt"}}
    }

    which_key.register(mappings, {mode = "n", prefix = "<leader>"})
end

return SPEC
```

### Cmp (nvim-cmp)

Completion engine with sources for LSP, snippets, buffer, path, emoji, and Copilot.

- Repository: [hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp)

```lua
local SPEC = {
    "hrsh7th/nvim-cmp",
    event = "InsertEnter",
    dependencies = {
        { "hrsh7th/cmp-nvim-lsp",    event = "InsertEnter" },
        { "hrsh7th/cmp-emoji",       event = "InsertEnter" },
        { "hrsh7th/cmp-buffer",      event = "InsertEnter" },
        { "hrsh7th/cmp-path",        event = "InsertEnter" },
        { "hrsh7th/cmp-cmdline",     event = "InsertEnter" },
        { "saadparwaiz1/cmp_luasnip",event = "InsertEnter" },
        { "L3MON4D3/LuaSnip",        event = "InsertEnter",
            dependencies = { "rafamadriz/friendly-snippets" } },
        { "hrsh7th/cmp-nvim-lua" }
    }
}

function SPEC.config()
    local cmp = require "cmp"
    local luasnip = require "luasnip"
    require("luasnip/loaders/from_vscode").lazy_load()

    vim.api.nvim_set_hl(0, "CmpItemKindCopilot", {fg = "#6CC644"})
    vim.api.nvim_set_hl(0, "CmpItemKindTabnine", {fg = "#CA42F0"})
    vim.api.nvim_set_hl(0, "CmpItemKindEmoji",   {fg = "#FDE030"})

    local check_backspace = function()
        local col = vim.fn.col "." - 1
        return col == 0 or vim.fn.getline("."):sub(col, col):match "%s"
    end

    local icons = require "kaekeshan.icons"

    cmp.setup {
        snippet = {
            expand = function(args)
                luasnip.lsp_expand(args.body)
            end
        },
        mapping = cmp.mapping.preset.insert {
            ["<C-k>"]       = cmp.mapping(cmp.mapping.select_prev_item(), {"i", "c"}),
            ["<C-j>"]       = cmp.mapping(cmp.mapping.select_next_item(), {"i", "c"}),
            ["<Down>"]      = cmp.mapping(cmp.mapping.select_next_item(), {"i", "c"}),
            ["<Up>"]        = cmp.mapping(cmp.mapping.select_prev_item(), {"i", "c"}),
            ["<C-b>"]       = cmp.mapping(cmp.mapping.scroll_docs(-1), {"i", "c"}),
            ["<C-f>"]       = cmp.mapping(cmp.mapping.scroll_docs(1),  {"i", "c"}),
            ["<C-Space>"]   = cmp.mapping(cmp.mapping.complete(),       {"i", "c"}),
            ["<C-e>"]       = cmp.mapping { i = cmp.mapping.abort(), c = cmp.mapping.close() },
            ["<CR>"]        = cmp.mapping.confirm {select = true},
            ["<Tab>"]       = cmp.mapping(
                function(fallback)
                    if cmp.visible() then
                        cmp.select_next_item()
                    elseif luasnip.expandable() then
                        luasnip.expand()
                    elseif luasnip.expand_or_jumpable() then
                        luasnip.expand_or_jump()
                    elseif check_backspace() then
                        fallback()
                    else
                        fallback()
                    end
                end,
                { "i", "s" }
            ),
            ["<S-Tab>"]     = cmp.mapping(
                function(fallback)
                    if cmp.visible() then
                        cmp.select_prev_item()
                    elseif luasnip.jumpable(-1) then
                        luasnip.jump(-1)
                    else
                        fallback()
                    end
                end,
                { "i", "s" }
            )
        },
        formatting = {
            fields = {"kind", "abbr", "menu"},
            format = function(entry, vim_item)
                vim_item.kind = icons.kind[vim_item.kind]
                vim_item.menu =
                    ({
                        nvim_lsp = "", nvim_lua = "", luasnip = "",
                        buffer = "", path = "", emoji = ""
                    })[entry.source.name]

                if entry.source.name == "emoji" then
                    vim_item.kind = icons.misc.Smiley
                    vim_item.kind_hl_group = "CmpItemKindEmoji"
                end

                if entry.source.name == "cmp_tabnine" then
                    vim_item.kind = icons.misc.Robot
                    vim_item.kind_hl_group = "CmpItemKindTabnine"
                end

                return vim_item
            end
        },
        sources = {
            {name = "copilot"},
            {name = "nvim_lsp"},
            {name = "luasnip"},
            {name = "cmp_tabnine"},
            {name = "nvim_lua"},
            {name = "buffer"},
            {name = "path"},
            {name = "calc"},
            {name = "emoji"}
        },
        confirm_opts = {behavior = cmp.ConfirmBehavior.Replace, select = false},
        window = {
            completion  = {border = "rounded", scrollbar = false},
            documentation = {border = "rounded"}
        },
        experimental = {ghost_text = false}
    }
end

return SPEC
```

### None-ls

Local formatting sources that don't need an LSP — runs stylua, prettier, black, clang-format, google-java-format on save.

- Repository: [nvimtools/none-ls.nvim](https://github.com/nvimtools/none-ls.nvim)

```lua
local SPEC = {
    "nvimtools/none-ls.nvim",
    dependencies = {
        "nvim-lua/plenary.nvim"
    }
}

function SPEC.config()
    local null_ls = require "null-ls"

    local formatting = null_ls.builtins.formatting
    local diagnostics = null_ls.builtins.diagnostics

    null_ls.setup {
        debug = false,
        sources = {
            formatting.stylua,
            formatting.prettier,
            formatting.black,
            formatting.clang_format,
            formatting.google_java_format
            -- formatting.prettier.with { extra_filetypes = { "toml" } },
            -- formatting.eslint,
            -- diagnostics.flake8,
        }
    }
end

return SPEC
```

## Reference

- [Ultimate Neovim Config | 2024 | Launch.nvim — YouTube](https://www.youtube.com/watch?v=KGJV0n70Mxs)