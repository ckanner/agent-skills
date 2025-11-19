# Kanner 的 Agent Skills 技能集

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

精心策划的 [Claude Agent Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills) 集合，旨在通过专业能力增强 AI 工作流。

**[中文](./README_CN.md)** | **[English](./README.md)**

## 🎯 可用技能

### 🚀 Prompt Optimizer（提示词优化器）

将用户提供的提示词转换为高质量、清晰且有效的指令，为 AI 模型进行优化。

**核心特性：**
- 系统化的提示词分析和优化
- 应用全面的提示词工程最佳实践
- 增强清晰度、具体性和结构性
- 改善 AI 模型的理解和执行效果

**使用场景：** 当你需要将模糊、不清晰或结构不良的提示词优化为有效指令时。

**位置：** [`prompt-optimizer/`](./prompt-optimizer/)

---

### 🌐 Jta（JSON 翻译 Agent）

基于 AI 的 JSON 国际化文件翻译器，具备 Agentic 反思机制，可生成高质量的多语言内容。

> **来源：** 此技能基于开源项目 [Jta](https://github.com/hikanner/jta)  
> **依赖要求：** 需要安装 `jta` 命令行工具（技能可自动安装）

**核心特性：**
- **Agentic 翻译**：AI 翻译、评估并改进自己的工作（每批 3 次 API 调用）
- **智能术语管理**：自动检测并保持术语一致性
- **格式保护**：保留 `{variables}`、`{{placeholders}}`、HTML 标签、URL、Markdown
- **增量模式**：仅翻译新增/变更内容（节省 80-90% API 成本）
- **27 种语言**：包括 RTL 语言（阿拉伯语、希伯来语、波斯语、乌尔都语）

**使用场景：** 当你需要翻译 JSON i18n/locale 文件、添加新语言或更新现有翻译时。

**位置：** [`jta/`](./jta/)

**额外设置：**
- 技能会自动检查并在需要时安装 `jta` 命令行工具
- 需要 OpenAI、Anthropic 或 Google Gemini 的 API 密钥
- 详细设置说明请参见 [Jta 文档](https://github.com/hikanner/jta)
- 技能专属文档请访问 [Jta Agent Skills 指南](https://github.com/hikanner/jta/blob/main/skills/README.md)

---

## 📦 安装方式

### 方法 1：通过命令行使用插件市场（推荐）

**在 Claude Code 中最简单的安装方式：**

```bash
# 步骤 1：添加插件市场
/plugin marketplace add hikanner/agent-skills

# 步骤 2：安装需要的技能
/plugin install prompt-optimizer@kanner-agent-skills
/plugin install jta@kanner-agent-skills

# 步骤 3：重启 Claude Code 以激活技能
```

**以交互方式浏览可用插件：**
```bash
/plugin
```

### 方法 2：团队配置（自动安装）

对于团队项目，在项目的 `.claude/settings.json` 中配置插件市场：

```json
{
  "extraKnownMarketplaces": {
    "kanner-agent-skills": {
      "source": {
        "source": "github",
        "repo": "hikanner/agent-skills"
      }
    }
  },
  "enabledPlugins": {
    "prompt-optimizer@kanner-agent-skills": true,
    "jta@kanner-agent-skills": true
  }
}
```

**当团队成员信任仓库文件夹时，Claude Code 会自动：**
- 安装指定的插件市场
- 安装并启用配置的插件
- 无需手动安装！

### 方法 3：本地开发

用于在发布前测试本地更改：

```bash
# 步骤 1：克隆仓库
git clone https://github.com/hikanner/agent-skills.git
cd agent-skills

# 步骤 2：添加为本地市场
/plugin marketplace add ./

# 步骤 3：安装插件
/plugin install prompt-optimizer@kanner-agent-skills
/plugin install jta@kanner-agent-skills
```

### 方法 4：直接文件安装

#### 个人用户（全局技能）

将技能复制到你的 Claude skills 目录：

```bash
# 克隆仓库
git clone https://github.com/hikanner/agent-skills.git

# 复制特定技能
cp -r agent-skills/prompt-optimizer ~/.claude/skills/
cp -r agent-skills/jta ~/.claude/skills/

# 或使用符号链接（推荐用于开发）
ln -s $(pwd)/agent-skills/prompt-optimizer ~/.claude/skills/prompt-optimizer
ln -s $(pwd)/agent-skills/jta ~/.claude/skills/jta
```

技能将在你的所有项目中可用。

#### 团队项目（项目技能）

将技能添加到项目的 `.claude/skills/` 目录：

```bash
# 在项目根目录
mkdir -p .claude/skills

# 复制需要的技能
cp -r /path/to/agent-skills/prompt-optimizer .claude/skills/
cp -r /path/to/agent-skills/jta .claude/skills/

# 提交到版本控制
git add .claude/skills
git commit -m "feat: 为团队添加 agent skills"
```

**团队成员克隆仓库时将自动获得这些技能** - 无需额外安装！

### 方法 4：Claude.ai 上传

在 Claude.ai 网页界面中使用：

1. 下载技能为 ZIP 文件：
   ```bash
   cd agent-skills
   zip -r prompt-optimizer.zip prompt-optimizer/
   zip -r jta.zip jta/
   ```

2. 在 Claude.ai 中：
   - 前往 **设置** → **功能**
   - 点击 **上传 Skill**
   - 选择 ZIP 文件
   - 启用技能

### 方法 5：API 使用

对于 API 用户，上传技能到工作区：

```python
from anthropic import Anthropic

client = Anthropic()

# 上传技能
with open("prompt-optimizer.zip", "rb") as f:
    skill = client.beta.skills.create(
        skill_file=f,
        betas=["skills-2025-10-02"]
    )

print(f"Skill ID: {skill.id}")
```

然后在 API 调用中使用：

```python
response = client.beta.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=4096,
    betas=["code-execution-2025-08-25", "skills-2025-10-02"],
    container={
        "skills": [
            {"type": "workspace", "skill_id": skill.id, "version": "latest"}
        ]
    },
    tools=[{"type": "code_execution_20250825", "name": "code_execution"}],
    messages=[{"role": "user", "content": "你的请求"}]
)
```

---

## 🔄 管理已安装的技能

### 在 Claude Code 中

使用 `/plugin` 命令管理已安装的技能：

```bash
# 获取可用插件命令的帮助
/plugin help

# 常用操作（请使用 /plugin help 验证您的版本支持的命令）
/plugin list              # 列出已安装的插件
/plugin install <名称>    # 安装插件
/plugin uninstall <名称>  # 卸载插件
```

**注意：** 具体的命令和选项可能因 Claude Code 版本而异。请始终使用 `/plugin help` 查看您安装版本的最新命令。

### 查看可用技能

在任何 Claude Code 对话中：

```
"有哪些技能可用？"
```

或

```
"列出所有可用的技能"
```

---

## 🚀 快速开始

### 使用 Prompt Optimizer

直接询问 Claude：

> "能帮我优化这个提示词以获得更好的结果吗？"

或

> "我需要改进这个指令：[你的提示词]"

### 使用 JTA

基础翻译：

> "将我的 locales/en.json 翻译成中文、日语和韩语"

增量更新：

> "我在 en.json 中添加了 5 个新键，请更新翻译"

CI/CD 设置：

> "在 GitHub Actions 中设置自动翻译"

---

## 📖 文档说明

### 什么是 Agent Skills？

[Agent Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills) 是扩展 AI 代理能力的模块化功能。它们由指令、脚本和资源组成，帮助代理自主执行专业任务。

### 技能结构

本仓库中的每个技能都遵循标准的 Agent Skills 格式：

```
skill-name/
├── SKILL.md          # 核心技能定义和指令
├── LICENSE.txt       # Apache 2.0 许可证
├── examples/         # （可选）分步用例
├── scripts/          # （可选）辅助脚本
└── references/       # （可选）附加文档
```

### 相关资源

- 📚 [Agent Skills 文档](https://docs.anthropic.com/docs/agents-and-tools/agent-skills)
- 💡 [Agent Skills 最佳实践](https://docs.anthropic.com/docs/agents-and-tools/agent-skills/best-practices)
- 🔧 [创建自定义 Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills/creating-skills)

---

## 🤝 贡献指南

欢迎贡献！如果你有新技能的想法或对现有技能的改进建议：

1. Fork 此仓库
2. 创建功能分支（`git checkout -b feature/new-skill`）
3. 提交你的更改（`git commit -m 'feat: add new skill'`）
4. 推送到分支（`git push origin feature/new-skill`）
5. 开启 Pull Request

### 贡献准则

- 遵循 [Agent Skills 规范](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
- 在 `SKILL.md` 中包含清晰的文档
- 在适用时添加示例
- 提交前进行充分测试

---

## 📜 许可证

本项目采用 Apache License 2.0 许可 - 详见 [LICENSE](./LICENSE) 文件。

每个技能的目录中都包含许可证副本（`LICENSE.txt`）。

---

## 🙏 致谢

- [Anthropic](https://www.anthropic.com/) 创建了 Claude 和 Agent Skills 框架
- 开源社区提供的灵感和贡献

---

## 📧 联系方式

**Kanner**  
📧 邮箱：kanner.chen@gmail.com

---

**为 Agent Skills 生态系统而生**

*让 AI 代理自动处理专业任务。*
