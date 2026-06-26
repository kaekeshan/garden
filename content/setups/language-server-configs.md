---
title: Neovim - LSP
draft: false
tags: [vim, lsp, reference]
date: 2025-12-01
---

The **Language Server Protocol (LSP)** is an open, JSON-RPC-based protocol between source-code editors / IDEs and language servers. It standardises "language intelligence" features — code completion, syntax highlighting, warnings, errors, refactors — so support for a language can be implemented once and reused across editors.

Reference: [wikipedia — Language Server Protocol](https://en.wikipedia.org/wiki/Language_Server_Protocol)

Below are example server configurations that hook into the `lspconfig` setup described in [[neo-vim|Neovim - Setup]]. Each file lives under `lua/user/lspsettings/<server>.lua` and is loaded dynamically by the main lspconfig block.

## lua_ls

Disables Lua formatter, registers common globals, points the workspace library at Neovim's runtime and your config, and turns off most hints.

```lua
-- https://luals.github.io/wiki/settings/
return {
    settings = {
        Lua = {
            format = {
                enable = false
            },
            diagnostics = {
                globals = {"vim", "spec"}
            },
            runtime = {
                version = "LuaJIT",
                special = {
                    spec = "require"
                }
            },
            workspace = {
                checkThirdParty = false,
                library = {
                    [vim.fn.expand "$VIMRUNTIME/lua"] = true,
                    [vim.fn.stdpath "config" .. "/lua"] = true
                }
            },
            hint = {
                enable = false,
                arrayIndex = "Disable",  -- "Enable" | "Auto" | "Disable"
                await = true,
                paramName = "Disable",   -- "All" | "Literal" | "Disable"
                paramType = true,
                semicolon = "All",       -- "All" | "SameLine" | "Disable"
                setType = false
            },
            telemetry = {
                enable = false
            }
        }
    }
}
```

## jsonls

Pulls schema definitions from `schemastore.nvim` (see [[neo-vim|Schemastore plugin]]) and adds a `:Format` command that runs over the full buffer.

```lua
return {
    settings = {
        json = {
            schemas = require("schemastore").json.schemas()
        }
    },
    setup = {
        commands = {
            Format = {
                function()
                    vim.lsp.buf.range_formatting({}, {0, 0}, {vim.fn.line "$", 0})
                end
            }
        }
    }
}
```

## jdtls

Eclipse JDT Language Server for Java. Uses Mason to resolve the install path, attaches Lombok's Java agent, and points the workspace at Java 8.

```lua
local nvim_lsp = require("lspconfig")
local install_path = require("mason-registry").get_package("jdtls"):get_install_path()

return {
    cmd = {
        "jdtls",
        install_path .. "/bin/jdtls",
        "--jvm-arg=-javaagent:" .. install_path .. "/lombok.jar",
        "-Declipse.application=org.eclipse.jdt.ls.core.id1",
        "-Dosgi.bundles.defaultStartLevel=4",
        "-Declipse.product=org.eclipse.jdt.ls.core.product",
        "-Dlog.protocol=true",
        "-Dlog.level=ALL",
        "-Xms1g",
        "--add-modules=ALL-SYSTEM",
        "--add-opens",
        "java.base/java.util=ALL-UNNAMED",
        "--add-opens",
        "java.base/java.lang=ALL-UNNAMED",
        "-data"
    },
    filetypes = {
        "java"
    },
    root_dir = nvim_lsp.util.root_pattern("pom.xml", "gradle.build", ".git"),
    settings = {
        java = {
            signatureHelp = {enabled = true},
            format = {enabled = true},
            codeGeneration = {
                toString = {template = "${object.className}{${member.name()}=${member.value}, ${otherMembers}}"}
            },
            sources = {
                organizeImports = {
                    starThreshold = 9999,
                    staticStarThreshold = 9999
                }
            },
            configuration = {
                runtimes = {
                    {
                        name = "JavaSE-1.8",
                        path = "C:\\Program Files\\Java\\jdk1.8.0_202\\bin",
                        default = true
                    }
                }
            }
        }
    }
}
```