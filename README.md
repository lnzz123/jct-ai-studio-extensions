# JCT AI Studio Extensions · 君城 AI 能力扩展源

JCT AI Studio 的官方能力扩展仓库。这里定义的能力会出现在平台「能力中心」中，管理员启用后自动下发到员工的 AI 工作台。客户部署时可将 `JCT_EXTENSIONS_REPO` 指向自己的仓库，实现定制能力上架。

## 目录结构

```
skills/<skill-name>/SKILL.md                          # 技能：操作规范、工作流、领域知识
mcp/<name>.json                                      # MCP 服务：远程 MCP endpoint + 认证字段
plugins/<plugin-name>/.plugin/plugin.json            # 插件：技能+命令+子代理组合包
```

## Skills

```markdown
---
name: my-skill
description: 一句话说明（必填，<=1024 字符）
---

# 技能正文：写给 AI 的操作指引
```

可选子目录：`scripts/`、`references/`、`assets/`。

## MCP 服务

每个 `mcp/*.json` 描述一个远程 MCP 服务：

```json
{
  "id": "internal-docs",
  "name": "内部文档",
  "description": "检索公司内部文档与知识库",
  "docsUrl": "https://docs.example.com",
  "connection": {
    "optionId": "default",
    "local": false,
    "target": "https://mcp.example.com/mcp",
    "transport": "streamable-http",
    "fields": [
      { "key": "token", "label": "访问令牌", "secret": true, "required": true, "secretName": "INTERNAL_DOCS_TOKEN" }
    ],
    "headers": { "X-Tenant": "jct" },
    "auth": { "strategy": "bearer", "secretName": "INTERNAL_DOCS_TOKEN" }
  }
}
```

- `fields[].secretName` 决定令牌存入能力中心密钥库的名字；管理员在商店里填写后自动加密保存。
- `auth` 支持 `bearer` / `api_key`（可配 `headerName`，默认 `X-API-Key`）；`headers` 为附加静态请求头。
- `transport` 可选 `streamable-http`（默认）或 `sse`。
- `local: true` 的条目不可用（公司能力不支持容器内命令型 MCP）。

## Plugins 插件

插件目录需包含 `.plugin/plugin.json`（name、description、keywords）以及可选的 `skills/`、`commands/`、`agents/` 子目录，格式与 OpenHands 官方插件一致。启用后随会话创建下发。

## 安全约定

- 不要提交真实密钥、内网拓扑、客户信息；令牌一律走能力中心 secrets。
- 技能内容会注入 AI 上下文，注意措辞准确、步骤可验证。
