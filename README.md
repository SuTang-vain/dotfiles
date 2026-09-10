# dotfiles

Personal scripts and machine setup, managed as plain files + symlinks.

## Layout

```
bin/figma-mcp   → symlinked to ~/.local/bin/figma-mcp
```

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
figma-mcp call get_figma_data '{"fileKey":"xxx","nodeId":"1:4"}'  # fetch design data
figma-mcp call download_figma_images '{"fileKey":"xxx","localPath":"./imgs","nodes":[{"nodeId":"1:4","fileName":"logo"}]}'
```
