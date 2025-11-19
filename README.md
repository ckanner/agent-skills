# Kanner's Agent Skills

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

A curated collection of [Claude Agent Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills) designed to enhance AI workflows with specialized capabilities.

**[English](./README.md)** ｜ **[中文](./README_CN.md)**

## 🎯 Available Skills

### 🚀 Prompt Optimizer

Transforms user-provided prompts into high-quality, clear, and effective instructions optimized for AI models.

**Key Features:**
- Systematic prompt analysis and optimization
- Applies comprehensive prompt engineering best practices
- Enhances clarity, specificity, and structure
- Improves AI model understanding and execution

**When to Use:** When you need to refine vague, unclear, or poorly structured prompts into effective instructions.

**Location:** [`prompt-optimizer/`](./prompt-optimizer/)

---

### 🌐 Jta (JSON Translation Agent)

AI-powered JSON internationalization file translator with agentic reflection mechanism for high-quality multilingual content.

> **Source:** This skill is based on the open-source [Jta project](https://github.com/hikanner/jta)  
> **Requirements:** Requires the `jta` CLI tool to be installed (the skill can auto-install it)

**Key Features:**
- **Agentic Translation**: AI translates, evaluates, and improves its own work (3x API calls per batch)
- **Smart Terminology**: Automatically detects and maintains consistent terms
- **Format Protection**: Preserves `{variables}`, `{{placeholders}}`, HTML tags, URLs, Markdown
- **Incremental Mode**: Only translates new/changed content (saves 80-90% API cost)
- **27 Languages**: Including RTL languages (Arabic, Hebrew, Persian, Urdu)

**When to Use:** When you need to translate JSON i18n/locale files, add new languages, or update existing translations.

**Location:** [`jta/`](./jta/)

**Additional Setup:**
- The skill will automatically check and install the `jta` CLI tool if needed
- Requires an API key from OpenAI, Anthropic, or Google Gemini
- See the [Jta documentation](https://github.com/hikanner/jta) for detailed setup instructions
- For skill-specific documentation, visit [Jta Agent Skills Guide](https://github.com/hikanner/jta/blob/main/skills/README.md)

---

## 📦 Installation

### Method 1: Plugin Marketplace via Command Line (Recommended)

**The easiest way to install in Claude Code:**

```bash
# Step 1: Add the marketplace
/plugin marketplace add hikanner/agent-skills

# Step 2: Install the skills you need
/plugin install prompt-optimizer@kanner-agent-skills
/plugin install jta@kanner-agent-skills

# Step 3: Restart Claude Code to activate the skills
```

**To browse available plugins interactively:**
```bash
/plugin
```

### Method 2: Team Configuration (Automatic Installation)

For team projects, configure the marketplace in your project's `.claude/settings.json`:

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

**When team members trust the repository folder, Claude Code automatically:**
- Installs the specified marketplace
- Installs and enables the configured plugins
- No manual installation needed!

### Method 3: Local Development

For testing local changes before publishing:

```bash
# Step 1: Clone the repository
git clone https://github.com/hikanner/agent-skills.git
cd agent-skills

# Step 2: Add as local marketplace
/plugin marketplace add ./

# Step 3: Install plugins
/plugin install prompt-optimizer@kanner-agent-skills
/plugin install jta@kanner-agent-skills
```

### Method 4: Direct File Installation

#### For Individual Users (Global Skills)

Copy skills to your Claude skills directory:

```bash
# Clone the repository
git clone https://github.com/hikanner/agent-skills.git

# Copy specific skills
cp -r agent-skills/prompt-optimizer ~/.claude/skills/
cp -r agent-skills/jta ~/.claude/skills/

# Or use symbolic links (recommended for development)
ln -s $(pwd)/agent-skills/prompt-optimizer ~/.claude/skills/prompt-optimizer
ln -s $(pwd)/agent-skills/jta ~/.claude/skills/jta
```

Skills will be available across all your projects.

#### For Project Teams (Project Skills)

Add skills to your project's `.claude/skills/` directory:

```bash
# In your project root
mkdir -p .claude/skills

# Copy the skills you need
cp -r /path/to/agent-skills/prompt-optimizer .claude/skills/
cp -r /path/to/agent-skills/jta .claude/skills/

# Commit to version control
git add .claude/skills
git commit -m "feat: add agent skills for team"
```

**Team members automatically get the skills when they clone the repository** - no additional installation needed!

### Method 4: Claude.ai Upload

For use in Claude.ai web interface:

1. Download skills as ZIP files:
   ```bash
   cd agent-skills
   zip -r prompt-optimizer.zip prompt-optimizer/
   zip -r jta.zip jta/
   ```

2. In Claude.ai:
   - Go to **Settings** → **Features**
   - Click **Upload Skill**
   - Select the ZIP file
   - Enable the skill

### Method 5: API Usage

For API users, upload skills to your workspace:

```python
from anthropic import Anthropic

client = Anthropic()

# Upload skill
with open("prompt-optimizer.zip", "rb") as f:
    skill = client.beta.skills.create(
        skill_file=f,
        betas=["skills-2025-10-02"]
    )

print(f"Skill ID: {skill.id}")
```

Then use in your API calls:

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
    messages=[{"role": "user", "content": "Your request"}]
)
```

---

## 🔄 Managing Installed Skills

### In Claude Code

Use the `/plugin` command to manage installed skills:

```bash
# Get help on available plugin commands
/plugin help

# Common operations (verify with /plugin help for your version)
/plugin list              # List installed plugins
/plugin install <name>    # Install a plugin
/plugin uninstall <name>  # Uninstall a plugin
```

**Note:** The exact commands and options may vary by Claude Code version. Always use `/plugin help` to see the most current commands for your installation.

### Check Available Skills

In any Claude Code conversation:

```
"What skills are available?"
```

or

```
"List all available skills"
```

---

## 🚀 Quick Start

### Using Prompt Optimizer

Simply ask Claude:

> "Can you help optimize this prompt for better results?"

or

> "I need to improve this instruction: [your prompt]"

### Using JTA

For basic translation:

> "Translate my locales/en.json to Chinese, Japanese, and Korean"

For incremental updates:

> "I added 5 new keys to en.json, please update the translations"

For CI/CD setup:

> "Set up automatic translation in GitHub Actions"

---

## 📖 Documentation

### What are Agent Skills?

[Agent Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills) are modular capabilities that extend what AI agents can do. They consist of instructions, scripts, and resources that help agents perform specialized tasks autonomously.

### Skill Structure

Each skill in this repository follows the standard Agent Skills format:

```
skill-name/
├── SKILL.md          # Core skill definition and instructions
├── LICENSE.txt       # Apache 2.0 license
├── examples/         # (Optional) Step-by-step use cases
├── scripts/          # (Optional) Helper scripts
└── references/       # (Optional) Additional documentation
```

### Related Resources

- 📚 [Agent Skills Documentation](https://docs.anthropic.com/docs/agents-and-tools/agent-skills)
- 💡 [Agent Skills Best Practices](https://docs.anthropic.com/docs/agents-and-tools/agent-skills/best-practices)
- 🔧 [Creating Custom Skills](https://docs.anthropic.com/docs/agents-and-tools/agent-skills/creating-skills)

---

## 🤝 Contributing

Contributions are welcome! If you have ideas for new skills or improvements to existing ones:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-skill`)
3. Commit your changes (`git commit -m 'feat: add new skill'`)
4. Push to the branch (`git push origin feature/new-skill`)
5. Open a Pull Request

### Guidelines

- Follow the [Agent Skills specification](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
- Include clear documentation in `SKILL.md`
- Add examples when applicable
- Test thoroughly before submitting

---

## 📜 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](./LICENSE) file for details.

Each skill includes a copy of the license in its directory (`LICENSE.txt`).

---

## 🙏 Acknowledgments

- [Anthropic](https://www.anthropic.com/) for creating Claude and the Agent Skills framework
- The open-source community for inspiration and contributions

---

## 📧 Contact

**Kanner**  
📧 Email: kanner.chen@gmail.com

---

**Made for the Agent Skills ecosystem**

*Enabling AI agents to handle specialized tasks automatically.*
