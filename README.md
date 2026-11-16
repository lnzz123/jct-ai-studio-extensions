# JCT Skills · 君城 AI 能力源

JCT AI Studio 的公司能力仓库。这里的技能（Skills）会出现在平台「能力中心 → 技能」的「君城能力」分类中，管理员启用后自动安装到每位员工的 AI 工作台。

## 目录结构

\`\`\`
skills/
  <skill-name>/
    SKILL.md          # 必需：YAML frontmatter + Markdown 正文
    scripts/          # 可选：可执行脚本
    references/       # 可选：参考文档
    assets/           # 可选：静态资源
\`\`\`

## SKILL.md 格式

\`\`\`markdown
---
name: my-skill
description: 一句话说明这个技能做什么、什么时候用（必填，<=1024 字符）
---

# 技能正文

写给 AI 的操作指引：步骤、命令、注意事项、验收标准。
\`\`\`

- \`name\` 缺省时使用目录名；不要包含密钥、内网地址等敏感信息。
- 技能更新由平台定时拉取（deploy/refresh-skills.sh），无需手动同步。

## MCP / 插件能力

API 类能力建议做成 MCP 服务，通过能力中心按条目配置；本仓库当前承载 Skills，后续可扩展 \`plugins/\`。
