## Figma 设计数据获取

本机已装 `figma-bridge`（Figma REST API 的 CLI，不依赖桌面客户端和 MCP，token 已配置）。
当用户给出 Figma 链接时：fileKey 是 URL 中 `/design/` 后的一段；node-id=1-4 写成 `"1:4"`。

**必须渐进式查询（先大纲后细节，控制 token 消耗）：**

```bash
figma-bridge screens <fileKey>                              # 1. 列页面/画板（输出很小）
figma-bridge node <fileKey> <nodeId> --depth 1              # 2. 定点取节点，先浅后深
figma-bridge node <fileKey> <nodeId> --depth 3 --fields layout+text   # 需要时加深/加字段
figma-bridge images <fileKey> <id1,id2> -o ./assets --scale 2        # 3. 只下载用到的素材
```

`--fields` 预设：`all`（默认）、`layout+text`、`content`、`visuals`、`layout`，用能满足任务的最窄档。
多步查询优先用批量模式：`figma-bridge nodejs <<'EOF' ... EOF`，预置 `getScreens`/`getNode`/`getImages`/`cliLog`，支持顶层 await。
完整说明见 ~/dotfiles/figma-bridge/SKILL.md。

## 浏览器自动化

本机已装 `ego-browser`（操作 ego lite 浏览器，复用用户登录态）：

```bash
ego-browser nodejs <<'EOF'
const task = await useOrCreateTaskSpace('任务名')
await openOrReuseTab('https://example.com', { wait: true })
cliLog(await snapshotText())
EOF
```

完整用法见 /Applications/ego lite.app/Contents/Resources/ego-browser/SKILL.md。
