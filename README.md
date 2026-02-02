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
- **Browser automation tracking** via Chrome extension
- **AI-powered command generation** (Anthropic, OpenAI, Ollama)
- **Multi-step workflows** with templated inputs

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

## Configuration

DevDeck uses TOML configuration at `~/.config/devdeck/devdeck.toml`.

### Layout

```toml
[layout]
columns = 4
landscape_columns = 6  # Optional: columns for landscape orientation
background_color = "#000000"
button_size = 60
```

### Server

```toml
[server]
port = 4242
```

### Logging

```toml
[log]
level = "info"        # debug, info, warn, error, fatal
file_enabled = true   # Write logs to ~/.config/devdeck/logs/
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
