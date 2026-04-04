# Everything Guide: GitHub Copilot CLI

Welcome to the comprehensive guide covering **everything** you need to know about GitHub Copilot CLI. This document serves as a complete reference for all features, commands, configuration options, and capabilities.

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [All Slash Commands](#all-slash-commands)
3. [All Configuration Options](#all-configuration-options)
4. [All Features](#all-features)
5. [All Integrations](#all-integrations)
6. [All Authentication Methods](#all-authentication-methods)
7. [All Platform Support](#all-platform-support)
8. [All Extension Points](#all-extension-points)
9. [Troubleshooting Everything](#troubleshooting-everything)

---

## Core Concepts

### What is GitHub Copilot CLI?

GitHub Copilot CLI brings AI-powered coding assistance directly to your command line. It provides:

- **Terminal-native development** - Work with Copilot coding agent directly in your command line
- **GitHub integration** - Access repositories, issues, and pull requests using natural language
- **Agentic capabilities** - Build, edit, debug, and refactor code with AI collaboration
- **MCP-powered extensibility** - Extend capabilities through Model Context Protocol servers
- **Full control** - Preview every action before execution

### Core Components

1. **Sessions** - Interactive conversation contexts that maintain state
2. **Agents** - Specialized AI assistants for different tasks
3. **Tools** - Built-in capabilities (file operations, git, search, etc.)
4. **MCP Servers** - External integrations via Model Context Protocol
5. **LSP Servers** - Language intelligence via Language Server Protocol
6. **Skills** - Reusable AI capabilities
7. **Plugins** - Extensions that add functionality
8. **Hooks** - Automation triggers for events

---

## All Slash Commands

Slash commands are the primary way to interact with special features in Copilot CLI.

### Session Management

- `/clear` - Abandon the current session entirely
- `/new` - Start a fresh conversation (keeps old session backgrounded)
- `/session` - Manage sessions
  - `/session rename [name]` - Rename session (auto-generates name if omitted)
  - `/session list` - List all sessions
  - `/session switch` - Switch between sessions
- `/resume` - Resume a previous session

### Mode Control

- `/experimental` - Enable experimental features
- Modes accessible via `Shift+Tab`:
  - **Normal mode** - Standard interaction
  - **Autopilot mode** - Agent continues working until task completion (experimental)

### Model Selection

- `/model` - Choose from available models:
  - Claude Sonnet 4.5 (default)
  - Claude Sonnet 4
  - GPT-5
  - Custom BYOM (Bring Your Own Model) providers
- Model picker includes inline reasoning effort adjustment with ← / → arrow keys

### Tool and Permission Management

- `/yolo` or `/allow-all` - Approve all tool uses without prompting
  - `/allow-all on` - Enable allow-all mode
  - `/allow-all off` - Disable allow-all mode
  - `/allow-all show` - Check current allow-all status

### History and Navigation

- `/rewind` or double-`Esc` - Open timeline picker to roll back conversation
- `/undo` - Undo recent changes
- `Ctrl+Y` - Open most recent research report (in plan mode)

### MCP Server Management

- `/mcp` - Manage Model Context Protocol servers
  - `/mcp show` - Display configured MCP servers
  - `/mcp list` - List available MCP servers
  - Supports OAuth authentication and Dynamic Client Registration
  - Organization policy enforcement for third-party servers

### Development Tools

- `/diff` - View code differences
- `/copy` - Copy content to clipboard
- `/cd` - Change working directory (per-session)
- `/lsp` - View Language Server Protocol status

### Extension Management

- `/plugin` - Manage plugins
  - Install/uninstall marketplace plugins
  - Plugin data stored per plugin
- `/skills` - Manage skills
  - Personal skills in `~/.agents/skills/`
  - Project skills discovered from working directory to git root
- `/instructions` - Manage custom instructions
- `/hooks` - Manage webhook/hook configuration

### Information and Help

- `/help` - Get help with using Copilot CLI
- `/feedback` - Submit confidential feedback survey
- `/tasks` - View current tasks

### Special Commands

- `/login` - Authenticate with GitHub
- `--banner` flag - Show animated banner on launch

---

## All Configuration Options

### Configuration File Locations

Copilot CLI supports multiple configuration layers:

#### User-Level Configuration
- `~/.copilot/` - Main configuration directory
- `~/.copilot/lsp-config.json` - LSP server configuration
- `~/.copilot/settings.json` - User settings
- `~/.agents/skills/` - Personal skills directory

#### Repository-Level Configuration
- `.github/lsp.json` - Project LSP configuration
- `.claude/settings.json` - Project settings
- `.claude/settings.local.json` - Local project settings
- `.mcp.json` - Workspace MCP server definitions

#### Environment Variables
- `GH_TOKEN` - GitHub personal access token
- `GITHUB_TOKEN` - Alternative GitHub token variable
- `CLAUDE_PROJECT_DIR` - Project directory (in hooks)
- `CLAUDE_PLUGIN_DATA` - Plugin data directory (in hooks)

### LSP Server Configuration

Configure Language Server Protocol servers for enhanced code intelligence:

```json
{
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".ts": "typescript",
        ".tsx": "typescript"
      }
    },
    "python": {
      "command": "pylsp",
      "args": [],
      "fileExtensions": {
        ".py": "python"
      }
    }
  }
}
```

Supported features:
- Go-to-definition
- Hover information
- Diagnostics
- Code completion

### MCP Server Configuration

MCP servers extend Copilot CLI capabilities:

```json
{
  "mcpServers": {
    "custom-server": {
      "command": "node",
      "args": ["./server.js"],
      "env": {
        "API_KEY": "value"
      }
    }
  }
}
```

Features:
- Custom tool definitions
- External API integrations
- LLM inference requests (with approval)
- OAuth authentication support
- Microsoft Entra ID authentication

### Hook Configuration

Hooks allow automation on events:

```json
{
  "hooks": {
    "sessionStart": {
      "command": "echo 'Session started'",
      "additionalContext": "Custom context to inject"
    },
    "user-prompt-submit": {
      "command": "validate-prompt.sh"
    }
  }
}
```

Template variables:
- `{{project_dir}}` - Project directory path
- `{{plugin_data_dir}}` - Plugin data directory path

Environment variables:
- `CLAUDE_PROJECT_DIR`
- `CLAUDE_PLUGIN_DATA`

### Command-Line Flags

- `--experimental` - Enable experimental features
- `--banner` - Show animated banner
- `--model <model>` - Override default model
- `--config-dir <path>` - Custom configuration directory
- `--alt-screen` - (Deprecated - always enabled)

---

## All Features

### Agentic Capabilities

- **Task Planning** - Agent can plan complex multi-step tasks
- **Code Generation** - Write new code based on requirements
- **Code Editing** - Modify existing code intelligently
- **Debugging** - Analyze and fix bugs
- **Refactoring** - Restructure code while maintaining functionality
- **Testing** - Generate and run tests

### Code Intelligence

- **Semantic Search** - Find code by meaning, not just text
- **Symbol Lookup** - Navigate to definitions
- **Call Graphs** - Understand function relationships
- **Class Hierarchies** - Explore inheritance structures
- **Code Summaries** - AI-generated documentation

### Git Integration

- **Repository Context** - Full git repository awareness
- **Branch Management** - Create, switch, merge branches
- **Commit Operations** - Stage, commit, push changes
- **Pull Requests** - Create and manage PRs
- **Issue Access** - Reference and link issues

### Terminal Features

- **Kitty Keyboard Protocol** - `Shift+Enter` for newlines
- **OSC 8 Hyperlinks** - Clickable links in supported terminals
- **Mouse Support** - Click and select text
- **Emoji Support** - Proper emoji selection and highlighting
- **Alt Screen** - Full screen terminal mode (always enabled)
- **Clipboard Integration** - Copy/paste support across platforms

### Performance Optimizations

- **V8 Compile Cache** - Faster startup through code caching
- **Parallel Operations** - Terminal detection, auth, git run in parallel
- **Streaming Optimization** - Reduced CPU usage during model streaming
- **Memory Efficiency** - Handles high-volume command output
- **Timeout Handling** - Grep/glob results return promptly

### Security Features

- **Trusted Folder Prompts** - Permission system for file access
- **Path Allowlists** - Control which paths can be accessed
- **Classic PAT Detection** - Clear error messages for incompatible tokens
- **Organization Policies** - MCP server allowlist enforcement
- **Token Scoping** - Fine-grained PAT with "Copilot Requests" permission

---

## All Integrations

### GitHub Integration

Built-in GitHub MCP server provides:
- Repository access
- Issue management
- Pull request operations
- GitHub Actions
- Project boards
- Discussions

### MCP Protocol

Model Context Protocol enables:
- Custom tool definitions
- External service integration
- Database connections
- API clients
- Authentication flows
- Dynamic resource loading

### LSP Protocol

Language Server Protocol provides:
- Syntax highlighting
- Error detection
- Auto-completion
- Refactoring tools
- Symbol navigation
- Hover documentation

### Browser Integration

- `$BROWSER` environment variable support
- Space-separated browser command parsing
- OAuth authentication flows
- Web-based authorization

### Extension Ecosystem

- **Marketplace Plugins** - Installable extensions
- **Custom Skills** - AI capability modules
- **Custom Agents** - Specialized AI assistants
- **Hook Scripts** - Event-driven automation

---

## All Authentication Methods

### GitHub Authentication

1. **OAuth Device Flow** - Interactive browser authentication
   ```bash
   copilot
   /login
   ```

2. **Personal Access Token (PAT)** - Fine-grained token
   - Create at: https://github.com/settings/personal-access-tokens/new
   - Required permission: "Copilot Requests"
   - Set via: `GH_TOKEN` or `GITHUB_TOKEN` environment variable

3. **Organization Access** - Enterprise/org authentication
   - Requires org/enterprise admin approval
   - Respects organization policies

### MCP Server Authentication

- **OAuth 2.0** - Standard OAuth flows
- **Dynamic Client Registration** - Automatic client setup
- **Microsoft Entra ID** - Azure AD integration (no repeated consent)
- **Custom Authentication** - Server-specific auth schemes

### Remote Host Authentication

- SSH key authentication
- GitHub MCP server user configuration respected
- Configuration persists across remote connections

---

## All Platform Support

### Operating Systems

#### Linux
- Debian/Ubuntu (apt)
- Fedora/RHEL (dnf/yum)
- Arch (pacman)
- Generic (curl/wget install)

#### macOS
- Intel (x86_64)
- Apple Silicon (arm64)
- Homebrew installation
- Direct download

#### Windows
- Windows 10/11
- PowerShell v6+ required
- WinGet installation
- npm installation
- Native clipboard support (no U+FEFF character)

### Architectures

- x86_64 / amd64
- ARM64 / aarch64

### Terminal Compatibility

- Kitty - Full support with keyboard protocol
- iTerm2 - Full support
- VS Code Terminal - OSC 8 hyperlinks supported
- Windows Terminal - Full support
- PowerShell - Native support
- Bash - Full support
- Zsh - Full support
- Fish - Full support

### File System Support

- POSIX file systems
- Windows NTFS
- OneDrive paths (Windows)
- Case-insensitive file systems
- Symbolic links

---

## All Extension Points

### Custom Skills

Location: `~/.agents/skills/` or project `skills/` directory

Skills are reusable AI capabilities that can be invoked:
```bash
/skill <skill-name>
```

### Custom Agents

Location: Project `.github/agents/` or user `~/.agents/`

Custom agents provide specialized AI assistants:
- Code review agents
- Documentation agents
- Test generation agents
- Deployment agents

### Plugins

Marketplace plugins extend functionality:
- Install: `/plugin install <name>`
- Uninstall: `/plugin uninstall <name>`
- Data directory: Per-plugin isolated storage

### Hooks

Event-driven automation:

**Available Hooks:**
- `sessionStart` - When session begins
- `user-prompt-submit` - Before processing user input
- `tool-call` - When tool is invoked
- `tool-result` - After tool completes

**Hook Configuration:**
- User hooks: `~/.copilot/hooks.json`
- Project hooks: `.github/hooks.json`
- Extension hooks: Merge with user/project hooks

### Custom Instructions

Location: Workspace or user configuration

Inject custom context into every conversation:
- Project-specific conventions
- Team guidelines
- Coding standards
- Preferred patterns

---

## Troubleshooting Everything

### Common Issues

#### Authentication Issues

**Symptom:** Cannot log in
- Check PAT has "Copilot Requests" permission
- Classic PATs are not supported
- Verify organization has not disabled CLI access

#### MCP Server Issues

**Symptom:** MCP servers not loading
- Check `.mcp.json` is in correct location (git root)
- Verify server command is in PATH
- Check organization allowlist policy
- Use `/mcp show` to debug

#### LSP Server Issues

**Symptom:** No code intelligence
- Install LSP server: `npm install -g typescript-language-server`
- Configure in `~/.copilot/lsp-config.json` or `.github/lsp.json`
- Use `/lsp` to check status

#### Performance Issues

**Symptom:** High CPU usage
- Update to latest version (spinner rendering optimized)
- Check for high-volume shell command output
- Enable V8 compile cache (automatic)

#### Permission Issues

**Symptom:** Unexpected permission prompts
- Check trusted folder configuration
- Windows OneDrive paths handled specially
- Case-insensitive filesystems require extra care

#### Session Issues

**Symptom:** Sessions not resuming correctly
- Verify `--config-dir` is consistent
- Check session not cleaned by stale reaper (fixed in recent versions)
- Ensure working directory restored with `/cd`

### Getting Help

1. **In-CLI Help:** `/help`
2. **Feedback:** `/feedback`
3. **GitHub Issues:** https://github.com/github/copilot-cli/issues
4. **Documentation:** https://docs.github.com/copilot/concepts/agents/about-copilot-cli
5. **Community:** GitHub Discussions

### Diagnostic Commands

- `/session` - Check session status
- `/mcp show` - View MCP servers
- `/lsp` - Check LSP status
- `/tasks` - View pending tasks
- `/model` - Check current model
- `/allow-all show` - Check permission mode

### Log Files

Location varies by platform:
- Linux: `~/.copilot/logs/`
- macOS: `~/Library/Application Support/copilot/logs/`
- Windows: `%APPDATA%/copilot/logs/`

### Version Checking

```bash
copilot --version
```

Keep updated for latest features and fixes!

---

## Advanced Topics

### Monorepo Support

Copilot CLI discovers configuration at every directory level from working directory to git root:
- Custom instructions
- MCP servers
- Skills
- Agents

Each subdirectory can have its own configuration that merges with parent configuration.

### Session Management

- Multiple concurrent sessions supported
- Session-specific working directory (preserved with `/cd`)
- Session history and timeline
- Resume previous sessions
- Background sessions with `/new`

### OpenTelemetry Integration

Hook executions recorded as span events in OTEL traces for observability.

### Reasoning Effort Control

Adjust model reasoning depth:
- Low - Fast responses
- Medium - Balanced (default)
- High - Deep analysis

Configure in model picker with ← / → arrow keys.

### Premium Requests

Each prompt consumes one premium request from monthly quota. See: https://docs.github.com/copilot/managing-copilot/monitoring-usage-and-entitlements/about-premium-requests

---

## Summary

This guide covered **everything** about GitHub Copilot CLI:

✓ All slash commands and their options
✓ All configuration files and locations
✓ All features and capabilities
✓ All integrations and extension points
✓ All authentication methods
✓ All supported platforms
✓ All troubleshooting approaches

For the most up-to-date information, see the [official documentation](https://docs.github.com/copilot/concepts/agents/about-copilot-cli) and [changelog](./changelog.md).

Happy coding with GitHub Copilot CLI! 🚀
