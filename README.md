# Universal Configs

A centralized repository for managing configuration files across work and personal machines. Currently tracking: Ghostty, Tmux, Zsh, Claude Code, and OpenCode.

## Quick Start

Clone this repo and copy files to your home directory:

```bash
git clone https://github.com/yourusername/configs.git ~/configs
cd ~/configs

# Create necessary directories
mkdir -p ~/.config/ghostty ~/.config

# Copy configuration files
cp config.ghostty ~/.config/ghostty/config
cp starship.toml ~/.config/starship.toml
cp tmux.conf ~/.tmux.conf
cp .zshrc ~/.zshrc
cp claude-code.json ~/.claude.json  # See notes below

# Reload shell
source ~/.zshrc
```

## Configuration Files

### Starship (`starship.toml`)
Modern shell prompt showing:
- **Git** branch, status, and changes
- **Kubernetes** current context and namespace
- **Docker** when in a Docker project
- **Language versions** (Python, Node.js, Ruby, Rust, Go)
- Clean, minimal design with emojis

**Key Features:**
- Shows only relevant info (auto-detects your project type)
- Fast command execution timeouts
- Works across all shells (zsh, bash, fish, etc.)

**Installation:**
```bash
# Install Starship (if not already installed)
curl -sS https://starship.rs/install.sh | sh

# Copy config
mkdir -p ~/.config
cp starship.toml ~/.config/starship.toml
```

**Example Prompt Output:**
```
➜ ~/projects/myapp
on main ✓ via ⬢ v18.0.0 via 🐍 v3.11.0
```

The prompt automatically adjusts based on your directory:
- Shows git info only in git repos
- Shows k8s context only when kubeconfig is detected
- Shows Docker info only in Docker projects
- Shows language versions only when detected

### Ghostty (`config.ghostty`)
Terminal emulator configuration with:
- Custom keybindings (Cmd+T for new tab, Cmd+N for new window)
- Dark theme with Catppuccin colors
- Search functionality (Cmd+F)

**Installation:**
```bash
mkdir -p ~/.config/ghostty
cp config.ghostty ~/.config/ghostty/config
```

### Tmux (`tmux.conf`)
Terminal multiplexer with:
- **Synchronize-panes**: `Cmd+Shift+S` to send commands to all panes in current window
- Split panes: `Cmd+D` (vertical), `Cmd+Shift+D` (horizontal)
- Navigation: `Cmd+Arrow keys` to move between panes
- New window: `Cmd+N`

**Installation:**
```bash
cp tmux.conf ~/.tmux.conf
tmux source-file ~/.tmux.conf
```

**Synchronize Panes Usage:**
```bash
# Toggle synchronize mode with Cmd+Shift+S
# All panes in the current window will receive your input
tmux setw synchronize-panes on
tmux setw synchronize-panes off
```

### Zsh (`.zshrc`)
Shell configuration with:
- Oh-My-Zsh integration
- Powerlevel10k prompt
- Kubernetes aliases (`k`, `kx`, `kubens`)
- Docker/AWS helpers
- FZF fuzzy finder integration
- NVM for Node.js management

**Installation:**
```bash
cp .zshrc ~/.zshrc
source ~/.zshrc
```

### Claude Code (`claude-code.json`)
Settings for Claude Code CLI.

**⚠️ Important:** The actual `.claude.json` file contains your user ID and authentication tokens. Do NOT commit your personal config. This file is a reference only.

To sync Claude Code settings:
1. Manually copy settings between machines via `~/.claude.json`
2. Or commit only non-sensitive parts of the config

### OpenCode
Configuration for OpenCode (development tools) - see `/opencode` directory.

## Machine Setup

To set up a new machine:

```bash
# Install required tools
brew install starship tmux ghostty

# Clone the configs repo
git clone <your-repo-url> ~/configs
cd ~/configs

# Create config directories
mkdir -p ~/.config/ghostty

# Install all configs
cp config.ghostty ~/.config/ghostty/config
cp starship.toml ~/.config/starship.toml
cp tmux.conf ~/.tmux.conf
cp .zshrc ~/.zshrc

# Reload shell
source ~/.zshrc

# Install Claude Code (manual - don't sync tokens)
# Copy only the non-sensitive parts of claude-code.json
```

### Minimal Setup (just the essentials)

```bash
# If you already have starship/tmux installed, just sync configs:
git clone <your-repo-url> ~/configs
cd ~/configs
cp starship.toml ~/.config/starship.toml
cp tmux.conf ~/.tmux.conf
cp .zshrc ~/.zshrc
source ~/.zshrc
```

## Keyboard Shortcuts Reference

### Ghostty
| Action | Shortcut |
|--------|----------|
| New Tab | `Cmd+T` |
| New Window | `Cmd+N` |
| Close Tab | `Cmd+W` |
| Switch Tab (prev) | `Cmd+[` |
| Switch Tab (next) | `Cmd+]` |
| Copy | `Cmd+C` |
| Paste | `Cmd+V` |
| Search | `Cmd+F` |

### Tmux
| Action | Shortcut |
|--------|----------|
| Split Vertical | `Cmd+D` |
| Split Horizontal | `Cmd+Shift+D` |
| Move Pane | `Cmd+Arrow` |
| Resize Pane | `Cmd+Alt+Arrow` |
| Sync Panes | `Cmd+Shift+S` |
| New Window | `Cmd+N` |
| Kill Pane | `Cmd+W` |
| New Session | `Cmd+N` (in tmux) |
| Reload Config | `Ctrl+Space + R` |

## Tips

1. **For pane synchronization across work**: Use `Cmd+Shift+S` in Tmux to enable `synchronize-panes`. Type once, and all panes execute the command.

2. **Keeping configs in sync**: Use git to version control and push changes:
   ```bash
   cd ~/configs
   git add .
   git commit -m "Update config for new machine"
   git push
   ```

3. **Machine-specific overrides**: Create a `.zshrc.local` file for machine-specific aliases/settings without committing them to git.

## Contributing

To update a config:
1. Make changes to your local files
2. Copy updated files to this repo: `cp ~/.tmux.conf ./tmux.conf`
3. Commit and push changes
4. Pull on other machines

## Directory Structure

```
configs/
├── config.ghostty        # Ghostty terminal emulator config
├── starship.toml         # Starship prompt config (git, k8s, docker, languages)
├── tmux.conf             # Tmux multiplexer config (pane sync, keybindings)
├── .zshrc                # Zsh shell config (aliases, paths, integration)
├── claude-code.json      # Claude Code reference config
├── opencode/             # OpenCode config files (if needed)
└── README.md             # This file
```

## Notes

- All configs use universal paths and should work on both macOS and Linux
- Some keybindings (like `Cmd`) are macOS-specific; adjust for your platform
- **Starship** requires the Nerd Font for proper emoji rendering. Install one:
  ```bash
  brew install font-meslo-lg-nerd-font
  # Then set it in your terminal (Ghostty, iTerm2, etc.)
  ```
- The `.zshrc` assumes Oh-My-Zsh is installed. Install it first if needed:
  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
- **Starship Performance**: The config includes timeouts to prevent slow prompts. Adjust `command_timeout` if needed.

## Starship Prompt Examples

Here's what you'll see in different scenarios:

**In a Git Repository:**
```
➜ ~/projects/my-app
on main !3 via ⬢ v18.0.0
```
Shows: directory, git branch, uncommitted changes, Node.js version

**In a Kubernetes Project:**
```
➜ ~/infrastructure/k8s-configs
on main ✓ on ☸ prod-cluster (default)
```
Shows: git status clean, Kubernetes cluster and namespace

**In a Python Project:**
```
➜ ~/ml-project
on dev !1 via 🐍 v3.11.2
```
Shows: git branch, modified files, Python version

**Multi-Language Project:**
```
➜ ~/fullstack-app
on develop !5 via ⬢ v20.0.0 via 🐍 v3.9.0
```
Shows: multiple language versions when detected

## Last Updated

2026-03-19
