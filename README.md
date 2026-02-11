# DevDeck Server

Official release binaries for DevDeck Server—the backend component of the DevDeck ecosystem.

## Overview

DevDeck Server facilitates communication between the DevDeck app and your computer, enabling custom control buttons for launching applications, running commands, and organizing workflows.

## Features

- **WebSocket-based communication** with bidirectional ping/pong keepalive
- **Command execution** with history tracking
- **Application launching** via macOS `open` command
- **mDNS service discovery** for automatic network detection
- **Configurable layout** with portrait/landscape orientation support
- **Context-based command grouping** with manual navigation
- **Context-aware deck switching** based on frontmost macOS app
- **Web UI** — browser-based command generation and settings at `localhost:4242/app`
- **TUI Generate tab** — AI generation directly in the terminal UI
- **AI-powered command generation** (Anthropic, OpenAI, Ollama)
- **Multi-step workflows** with templated inputs
- **Browser automation** via Chrome extension
- **Proxy/ngrok support** for remote access

## Installation

```bash
brew tap devdeck-app/homebrew-devdeck-server
brew install --cask devdeck-server
```

## Running

Start manually:

```bash
devdeck-server
```

Or as a background service:

```bash
brew services start devdeck-server
```

Server starts on port **4242** by default.

## Web UI

Access the built-in web interface at `http://localhost:4242/app`.

**Two tabs:**

| Tab | Description |
|-----|-------------|
| **Generate** | Type a natural language prompt → preview the AI-generated result → edit fields → save to your deck |
| **Settings** | Configure logging, AI provider, layout, proxy, context detection, history, and server options |

The Generate tab includes a **Deck Simulator** — a live grid preview of your deck layout that shows ghost buttons for unsaved commands before you commit them.

Localhost-only, no authentication required.

## TUI Generate Tab

Tab **5** in the terminal UI provides the same AI generation flow without leaving the terminal.

**Flow:** type prompt → preview result → edit fields → save to config

| Key | Action |
|-----|--------|
| `Enter` | Submit prompt |
| `s` | Save generated command/context/workflow |
| `e` | Toggle edit mode |
| `r` | Regenerate with same prompt |
| `Esc` | Cancel / discard preview |
| `Tab` / `Shift+Tab` | Cycle editable fields |

## Configuration

DevDeck uses TOML configuration at `~/.config/devdeck/devdeck.toml`.

### Server

```toml
[server]
port = 4242
```

### Layout

```toml
[layout]
columns = 4
landscape_columns = 6  # Optional: columns for landscape orientation
background_color = "#000000"
button_size = 60
```

### Commands

```toml
[[commands]]
uuid = "unique-id"          # Optional; auto-generated if not provided
description = "Launch Application"
app = "Application Name"
action = "open"
icon = "icon-name"          # SF Symbol name (e.g., "terminal.fill", "gear")
type = "action"             # Or "context" for sub-commands
context = "main"            # Or custom context name
main = true                 # Show on main screen
```

### AI Provider

Generate commands, contexts, and workflows from natural language using the Web UI, TUI Generate tab, or mobile app.

```toml
[ai]
provider = "anthropic"  # "anthropic" (default), "openai", or "ollama"
model = ""              # Optional; uses provider default if omitted
form_model = ""         # Optional; cheaper model for form field analysis
host = ""               # Ollama only (default: http://localhost:11434)
```

**Provider defaults:**
- **Anthropic**: `claude-sonnet-4-20250514` — requires `ANTHROPIC_API_KEY` env var
- **OpenAI**: `gpt-4o` — requires `OPENAI_API_KEY` env var
- **Ollama**: `qwen2.5-coder:7b` — local, no API key needed

`form_model` uses a separate (typically cheaper/faster) model for `fill_form` field analysis. Falls back to the main model if not set.

**Debug AI generation:**

```bash
DEVDECK_AI_LOG=true devdeck-server
```

### Context-Aware Deck Switching

Automatically switch decks based on the frontmost macOS application:

```toml
[settings]
auto_context_enabled = true      # Enable automatic context switching
auto_context_poll_ms = 500       # Detection poll interval (milliseconds)

[contexts.default]
name = "Default"
deck = "main"
priority = 0

[contexts.slack]
name = "Slack"
deck = "slack"
priority = 50
[contexts.slack.match]
frontmost_bundle_ids = ["com.tinyspeck.slackmacgap"]
frontmost_app_names = ["Slack"]
```

Priority-based matching — highest priority context wins. Manual override available via `set_manual_context` WebSocket event.

### Workflows

Multi-step automations with user input and templating:

```toml
[workflows.search-docs]
name = "Search Docs"
description = "Search documentation sites"
icon = "magnifyingglass"
inputs = ["query"]

[[workflows.search-docs.steps]]
type = "open_url"
url = "https://docs.example.com/search?q={{query}}"

[[workflows.search-docs.steps]]
type = "run_command"
command = "echo 'Opened docs for {{query}}'"
timeout = 10
continue_on_error = true
```

**Step types:**

| Type | Description |
|------|-------------|
| `open_url` | Opens URL in browser. Routes through extension when connected. |
| `run_command` | Executes shell command (default 30s timeout) |
| `delay` | Pauses execution (seconds) |
| `click_element` | Clicks DOM element by CSS selector (requires extension) |
| `fill_input` | Fills single input by CSS selector (requires extension) |
| `fill_form` | Fills form fields by semantic name (requires extension) |

Template variables use `{{inputName}}` syntax.

#### Form Filling (`fill_form`)

Fills web forms using semantic field names — no CSS selectors needed. Uses a hybrid approach: heuristic matching first (aria-labels, name attributes, placeholders), with AI fallback for ambiguous fields.

```toml
# Single-field: auto-submits by default
[workflows.ask-gemini]
name = "Ask Gemini"
icon = "sparkles"
inputs = ["query"]

[[workflows.ask-gemini.steps]]
type = "open_url"
url = "https://gemini.google.com/"

[[workflows.ask-gemini.steps]]
type = "fill_form"
[workflows.ask-gemini.steps.fields]
prompt = "{{query}}"

# Multi-field: does NOT auto-submit by default
[workflows.contact-form]
name = "Contact Form"
icon = "envelope"
inputs = ["name", "email", "message"]

[[workflows.contact-form.steps]]
type = "open_url"
url = "https://example.com/contact"

[[workflows.contact-form.steps]]
type = "fill_form"
auto_submit = false
[workflows.contact-form.steps.fields]
name = "{{name}}"
email = "{{email}}"
message = "{{message}}"
```

**`auto_submit` behavior:**
- Not set → smart default: auto-submit single-field forms, skip for multi-field
- `true` → always submit after filling
- `false` → never submit

### Proxy / ngrok

Enable automatic ngrok tunnel detection for remote access:

```toml
[proxy]
enable = true
```

When enabled, the server checks for a running ngrok tunnel and starts one if not found. The QR code and connection URL will use the ngrok public URL automatically.

Alternatively, set a manual external URL:

```toml
[settings]
manual_url = "example.ngrok-free.dev"
```

### Logging

```toml
[log]
level = "info"        # debug, info, warn, error, fatal
file_enabled = true   # Write logs to ~/.config/devdeck/logs/
```

### History

Command execution history is stored in `~/.config/devdeck/devdeck.db`. By default, only metadata (success, exit code, duration) is stored — command output is omitted for security.

```toml
[history]
store_output = true   # Enable storing command output (use with caution)
```

### Browser Extension

The Chrome extension enables web app detection for context-aware switching and browser automation (click elements, fill inputs, fill forms with semantic field mapping, navigate URLs):

```bash
# Load as unpacked extension in Chrome → Extensions → Developer Mode
# Extension located at tools/browser-extension/ in the server repo
```

```toml
[contexts.github]
name = "GitHub"
deck = "github"
priority = 60
[contexts.github.match]
frontmost_bundle_ids = ["com.google.Chrome"]
browser_url_patterns = ["*://github.com/*"]
```

## Examples

### Opening an Application

```toml
[[commands]]
app = "Obsidian"
icon = "doc.text"
action = "open"
type = "action"
main = true
```

### Creating a Context with Sub-commands

```toml
[[commands]]
app = "System Preferences"
type = "context"
icon = "gearshape.fill"
context = "system"
main = true

[[commands]]
description = "Volume Down"
action = "aerospace volume down"
type = "action"
context = "system"

[[commands]]
description = "Volume Up"
action = "aerospace volume up"
type = "action"
context = "system"
```

## License

[MIT License](LICENSE)
