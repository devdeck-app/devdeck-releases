# DevDeck Server Releases

Official release binaries for **DevDeck Server**—the backend component that powers the DevDeck mobile app.

## What is DevDeck?

DevDeck turns your phone into a customizable macro pad for your development workflow. Create custom control buttons for launching applications, running commands, and organizing workflows through a configurable interface.

## Installation

```bash
brew tap devdeck-app/homebrew-devdeck-server
brew install devdeck-server
```

## Running

Start the server manually:

```bash
devdeck-server
```

Or run it as a background service:

```bash
brew services start devdeck-server
```

The server starts on port **4242** by default.

## Configuration

DevDeck uses a TOML configuration file at `~/.config/devdeck/devdeck.toml`.

### Layout

```toml
[layout]
columns = 4
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
description = "Launch Application"
app = "Application Name"
action = "open"
icon = "icon-name"          # Icon from https://ionic.io/ionicons
type = "action"             # Or "context" for sub-commands
main = true                 # Show on main screen
```

## Examples

### Opening an Application

```toml
[[commands]]
app = "Obsidian"
icon = "document-outline"
action = "open"
type = "action"
main = true
```

### Creating a Context with Sub-commands

```toml
[[commands]]
app = "System Preferences"
type = "context"
icon = "cog-outline"
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
