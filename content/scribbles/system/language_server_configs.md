
---
title: Language Servers
draft: false
tags: ['vim','neovim','lua','lsp']
date: 2025-11-30
---

- LSP - Language Server Protocol Configurations are listed here
	- lua_ls
	  logseq.order-list-type:: number
	  collapsed:: true
		- ```
		  -- https://luals.github.io/wiki/settings/
		  return {
		    settings = {
		      Lua = {
		        format = {
		          enable = false,
		        },
		        diagnostics = {
		          globals = { "vim", "spec" },
		        },
		        runtime = {
		          version = "LuaJIT",
		          special = {
		            spec = "require",
		          },
		        },
		        workspace = {
		          checkThirdParty = false,
		          library = {
		            [vim.fn.expand "$VIMRUNTIME/lua"] = true,
		            [vim.fn.stdpath "config" .. "/lua"] = true,
		          },
		        },
		        hint = {
		          enable = false,
		          arrayIndex = "Disable", -- "Enable" | "Auto" | "Disable"
		          await = true,
		          paramName = "Disable", -- "All" | "Literal" | "Disable"
		          paramType = true,
		          semicolon = "All", -- "All" | "SameLine" | "Disable"
		          setType = false,
		        },
		        telemetry = {
		          enable = false,
		        },
		      },
		    },
		  }
		  ```
	- jsonls
	  logseq.order-list-type:: number
	  collapsed:: true
		- ```
		  return {
		    settings = {
		      json = {
		        schemas = require("schemastore").json.schemas(),
		      },
		    },
		    setup = {
		      commands = {
		        Format = {
		          function()
		            vim.lsp.buf.range_formatting({}, { 0, 0 }, { vim.fn.line "$", 0 })
		          end,
		        },
		      },
		    },
		  }
		  ```
	- jdtls
	  logseq.order-list-type:: number
	  collapsed:: true
		- ```
		  local nvim_lsp = require("lspconfig")
		  local install_path = require("mason-registry").get_package("jdtls"):get_install_path();
		  
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
		      "-data",
		    },
		    filetypes = {
		      "java"
		    },
		    root_dir = nvim_lsp.util.root_pattern('pom.xml', 'gradle.build', '.git'),
		    settings = {
		      java = {
		        signatureHelp = { enabled = true },
		        format = { enabled = true },
		        codeGeneration = { toString = { template = '${object.className}{${member.name()}=${member.value}, ${otherMembers}}' } },
		        sources = {
		          organizeImports = {
		            starThreshold = 9999,
		            staticStarThreshold = 9999
		          }
		        },
		      configuration = {
		          runtimes = {
		            {
		              name = 'JavaSE-1.8',
		              path = 'C:\\Program Files\\Java\\jdk1.8.0_202\\bin',
		              default = true
		            }
		          }
		        },
		      },
		    }
		  }
		  ```
