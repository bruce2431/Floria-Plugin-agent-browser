# Floria-Plugin-agent-browser

Floria 的浏览器自动化插件：直接启动 [agent-browser](https://github.com/vercel-labs/agent-browser) CLI 自带的 MCP stdio 服务，复用系统已安装的 Chrome/Edge，无需下载托管 Chromium。

## 要求

- Node.js >= 22
- 系统已安装 Chrome 或 Edge

## 安装

把本目录放到 `~/.claude/plugins/agent-browser/`（或工作区 `.claude/plugins/agent-browser/`），然后安装依赖：

```bash
npm install
```

插件通过 `.mcp.json` 注册 MCP 服务：

```json
{
  "mcpServers": {
    "browser": {
      "command": "node",
      "args": [
        "${CLAUDE_PLUGIN_ROOT}/node_modules/agent-browser/bin/agent-browser.js",
        "mcp",
        "--tools",
        "core"
      ],
      "env": {
        "AGENT_BROWSER_IDLE_TIMEOUT_MS": "600000"
      }
    }
  }
}
```

## 工具

`core` profile 共 33 个工具，名称形如 `mcp__plugin_agent-browser_browser__agent_browser_*`：

- 导航与快照：`agent_browser_open`、`agent_browser_snapshot`、`agent_browser_read`
- 交互：`agent_browser_click`、`agent_browser_fill`、`agent_browser_type`、`agent_browser_press`、`agent_browser_check`、`agent_browser_uncheck`、`agent_browser_select`、`agent_browser_scroll`
- 等待：`agent_browser_wait_ms`、`agent_browser_wait_for_selector`、`agent_browser_wait_for_text`、`agent_browser_wait_for_load`
- 读取与截图：`agent_browser_get_text`、`agent_browser_get_url`、`agent_browser_get_title`、`agent_browser_screenshot`、`agent_browser_eval`
- 标签页：`agent_browser_tab_new`、`agent_browser_tab_list`、`agent_browser_tab_switch`、`agent_browser_tab_close`、`agent_browser_back`、`agent_browser_forward`、`agent_browser_reload`
- 其它：`agent_browser_flow_list`、`agent_browser_flow_steps`、`agent_browser_flow_run`、`agent_browser_flow_save`、`agent_browser_tools_profiles`、`agent_browser_close`

需要更多能力时，把 `--tools core` 换成 `core,state,network,tabs,debug,mobile` 或 `all`。

## 许可

插件本身：ISC。底层 `agent-browser`：Apache-2.0。
