# dotfiles

Personal scripts and machine setup, managed as plain files + symlinks.

## Layout

```
bin/figma-mcp       → symlinked to ~/.local/bin/figma-mcp
agents/pi/AGENTS.md → symlinked to ~/.pi/agent/AGENTS.md
```

## figma-bridge

The Figma CLI now lives in its own repo: [SuTang-vain/figma-bridge](https://github.com/SuTang-vain/figma-bridge)
(faster, cached, no MCP layer — supersedes figma-mcp for most use).

## figma-mcp

CLI bridge to the [Framelink Figma MCP](https://github.com/Framelink/figma-developer-mcp) server
(REST-API based, no Figma desktop app needed). Requires the `mcptools` CLI (`brew install mcptools`).

```bash
# Symlink after cloning:
mkdir -p ~/.local/bin && ln -s "$(pwd)/bin/figma-mcp" ~/.local/bin/figma-mcp

# Configure the API key (Figma → Settings → Security → Personal access tokens):
# 1. $FIGMA_API_KEY env var, or
# 2. ~/.config/figma/api-key file (chmod 600 — never commit it)
mkdir -p ~/.config/figma && chmod 700 ~/.config/figma
printf 'YOUR_TOKEN' > ~/.config/figma/api-key && chmod 600 ~/.config/figma/api-key
```

Usage:

```bash
figma-mcp tools                                                  # list server tools
figma-mcp url '<figma-url>'                                      # extract fileKey/nodeId from a URL
figma-mcp data '<figma-url|fileKey>' [nodeId]                    # fetch design data (shortcut)
figma-mcp images '<figma-url|fileKey>' ./imgs '1:4' '2:9'        # download SVG/PNG assets (shortcut)
figma-mcp call get_figma_data '{"fileKey":"xxx","nodeId":"1:4"}'  # raw tool call
```

URLs like `https://www.figma.com/design/<key>/Name?node-id=1-4` are parsed
automatically — paste the link and the bridge extracts `fileKey` and `nodeId`.
