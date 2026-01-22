# Modern Dotfiles

Personal dotfiles with modern shell tooling, optimized for Laravel/PHP development. Features fast startup times, smart directory navigation, and unified version management.

## Quick Start

```bash
git clone git@github.com:freekmurze/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
bin/install
```

After installation:

```bash
bin/doctor                           # Verify installation
migration/migrate-z-to-zoxide.sh    # Only if upgrading from old setup
```

## Claude Code Only

If you only want Claude Code setup without the full dotfiles:

```bash
# Quick install (no dotfiles clone needed)
curl -fsSL https://raw.githubusercontent.com/freekmurze/dotfiles/main/bin/install-claude-code | bash

# Or with dotfiles configuration
git clone git@github.com:freekmurze/dotfiles.git ~/.dotfiles
~/.dotfiles/bin/install-claude-code
```

This installs:
- Claude Code CLI
- Custom configuration (CLAUDE.md, laravel-php-guidelines.md)
- Custom skills (ray-skill, fix-github-issue, convert-issue-to-discussion)
- Community skills (web design, React, React Native, marketing)

## What's Included

### Modern Shell Tools

**Starship** - Fast, customizable prompt that replaces Oh My Zsh themes
**zoxide** - Smart directory jumping based on frecency
**mise** - Unified version management for Node, PHP, and Ruby
**bat** - Cat with syntax highlighting
**eza** - Modern ls replacement with icons
**ripgrep** - Fast grep alternative
**fd** - Fast find alternative
**git-delta** - Better git diffs
**fzf** - Fuzzy finder for files and history
**direnv** - Automatic environment variables per directory

### Development Tools

**PHP** 8.4 managed via mise
**Node.js** LTS managed via mise
**Ruby** latest managed via mise
**Composer** with global packages
**Laravel Valet** for local development
**MySQL** with auto-start

### Shell Configuration

All aliases, functions, and environment variables are organized in the `home/` directory. Personal customizations go in `~/.dotfiles-custom/shell/` and won't be committed.

## Directory Structure

```
~/.dotfiles/
├── README.md          # This file
├── .gitignore
│
├── bin/               # Executable scripts
│   ├── install        # Main installer
│   ├── install-claude-code  # Standalone Claude Code installer
│   ├── update         # Update all packages and tools
│   └── doctor         # Health check and diagnostics
│
├── home/              # Home directory files
│   ├── .zshrc         # Zsh config (with Oh My Zsh) → symlinked to ~/
│   ├── .zshrc.no-omz  # Alternative config (faster, no Oh My Zsh)
│   ├── .aliases       # Command aliases (sourced by .zshrc)
│   ├── .functions     # Shell functions (sourced by .zshrc)
│   ├── .exports       # Environment variables (sourced by .zshrc)
│   ├── .vimrc         # Vim config → symlinked to ~/
│   ├── .vim/          # Vim runtime → symlinked to ~/
│   └── .global-gitignore → symlinked to ~/
│
├── config/            # Application configurations
│   ├── Brewfile       # Declarative package management
│   ├── starship.toml  # Starship prompt config
│   ├── .mise.toml     # Version manager config
│   ├── claude/        # Claude Code configuration
│   │   ├── CLAUDE.md
│   │   ├── laravel-php-guidelines.md
│   │   ├── settings.json
│   │   └── skills/
│   ├── fonts/         # Terminal fonts
│   │   └── Menlo-Powerline.otf
│   └── iterm/         # Terminal themes
│       ├── photon-white.itermcolors
│       ├── Solarized Dark Corrected.itermcolors
│       └── Solarized Dark xterm-256color patched.terminal
│
├── macos/             # macOS-specific
│   ├── .mackup.cfg
│   └── set-defaults.sh
│
└── migration/         # One-time migration scripts
    └── migrate-z-to-zoxide.sh
```

## Usage

### Daily Commands

```bash
z dotfiles          # Jump to frequently used directories
zi                  # Interactive directory picker
Ctrl+R              # Fuzzy search command history
Ctrl+T              # Fuzzy find files
Alt+C               # Fuzzy change directory
```

### Laravel/PHP Aliases

```bash
a                   # php artisan
p                   # Run Pest/PHPUnit tests
c                   # composer
mfs                 # php artisan migrate:fresh --seed
nah                 # git reset --hard; git clean -df
```

### Maintenance

```bash
bin/update                # Update dotfiles, Homebrew, mise, npm, composer
bin/doctor                # Verify installation and check for issues
bin/install-claude-code   # (Re)install or update Claude Code setup
```

### Version Management

Change language versions:

```bash
mise use node@20    # Switch to Node 20
mise use php@8.3    # Switch to PHP 8.3
mise current        # Show current versions
```

## Performance Options

### Default Configuration (with Oh My Zsh)

The default configuration uses Oh My Zsh for compatibility and its large plugin ecosystem. Startup time is approximately 200ms.

### Alternative: Without Oh My Zsh

For faster shell startup (approximately 50ms), switch to the alternative configuration:

```bash
ln -sf ~/.dotfiles/shell/.zshrc.no-omz ~/.zshrc
exec zsh
```

This uses native Zsh features and maintains all your aliases and functions. Switch back anytime:

```bash
ln -sf ~/.dotfiles/shell/.zshrc ~/.zshrc
exec zsh
```

## Package Management

### Adding New Tools

All Homebrew packages are declared in `config/Brewfile`. To add a new tool:

```bash
echo 'brew "neovim"' >> config/Brewfile
brew bundle --file=~/.dotfiles/config/Brewfile
```

### Managing Versions

Language versions are configured in `config/.mise.toml`:

```toml
[tools]
node = "lts"
php = "8.4"
ruby = "3.3"
```

## Customization

### Personal Aliases

Create custom aliases that won't be committed to git:

```bash
mkdir -p ~/.dotfiles-custom/shell
vim ~/.dotfiles-custom/shell/.aliases
```

### Project-Specific Environment Variables

Use direnv to automatically load environment variables per project:

```bash
cd my-project
echo 'export DEBUG=true' > .envrc
direnv allow
```

Variables are automatically loaded when you enter the directory and unloaded when you leave.

## Agent Skills

Agent Skills is an open ecosystem for extending Claude Code with community-created skill packages. Skills provide specialized knowledge and capabilities for various frameworks and tools.

### Pre-installed Skills

The installation automatically adds these community skills:

**Development & Design:**
- `vercel-labs/agent-skills` - Web design guidelines and React best practices
- `anthropics/skills` - Frontend design and skill creation tools
- `vercel-labs/agent-browser` - Browser automation capabilities

**Mobile Development:**
- `expo/skills` - React Native development with Expo
- `callstackincubator/agent-skills` - React Native performance best practices

**Marketing:**
- `coreyhaines31/marketingskills` - Copywriting and programmatic SEO

### Custom Skills

Your custom skills are stored in `config/claude/skills/` and include:
- `ray-skill` - Ray debugging integration
- `fix-github-issue` - GitHub issue automation
- `convert-issue-to-discussion` - GitHub workflow helpers

These are managed through your dotfiles and synced across machines.

### Adding More Skills

Browse available skills at [skills.sh](https://skills.sh) and add them anytime:

```bash
npx skills add <owner/repo>
```

## Post-Installation Steps

1. **Install fonts**: Double-click `config/fonts/Menlo-Powerline.otf` to install

2. **Import terminal theme**: In iTerm2, import `config/iterm/Solarized Dark Corrected.itermcolors`

3. **Verify installation**: Run `bin/doctor` to check everything is working

4. **Restore settings** (optional): If you have Mackup backups, run `mackup restore`

5. **Migrate directory history** (upgrading only): If you have `~/.z` from old setup, run `migration/migrate-z-to-zoxide.sh`

## Troubleshooting

### Shell is slow

Try the no-Oh-My-Zsh version for 4x faster startup:

```bash
ln -sf ~/.dotfiles/shell/.zshrc.no-omz ~/.zshrc
exec zsh
```

### Command not found after installation

```bash
exec zsh            # Reload shell
bin/doctor          # Check what's missing
mise doctor         # Verify mise installation
```

### Starship prompt not showing

```bash
starship --version  # Verify installation
which starship      # Check location
echo $STARSHIP_CONFIG  # Should point to config/starship.toml
```

### mise not switching versions

```bash
mise doctor         # Check for issues
mise current        # Show active versions
mise list           # Show installed versions
```

## Key Features Comparison

### Tool Replacements

| Old Tool | New Tool | Improvement |
|----------|----------|-------------|
| Oh My Zsh themes | Starship | 10x faster, highly customizable |
| z.sh / autojump | zoxide | Smarter ranking, Rust-based speed |
| nvm, fnm, rbenv | mise | Single tool for all languages |
| cat | bat | Syntax highlighting, git integration |
| ls | eza | Icons, colors, git status |
| grep | ripgrep | 5-10x faster, respects .gitignore |
| find | fd | Faster, simpler syntax |
| diff | delta | Side-by-side, syntax highlighting |

### Installation Improvements

**Before**
- 207 lines of installation script
- 50+ individual brew install commands
- Separate bootstrap and installscript files
- Manual version checking
- No health check or update mechanism

**After**
- 155 lines (25% reduction)
- Single unified `bin/install` script
- Declarative Brewfile (including Claude Code)
- Automatic health checks with `bin/doctor`
- One-command updates with `bin/update`
- Standalone Claude Code installer
- Agent skills pre-installed
- Optional performance configuration

## What Gets Installed

### Core Tools

node, pkg-config, wget, httpie, ncdu, hub, ack, doctl, 1password-cli, git-secret, imagemagick, mysql, yarn, ghostscript, mackup

### Modern CLI Tools

starship, zoxide, bat, eza, ripgrep, fd, git-delta, mise, fzf, direnv

### QuickLook Plugins

qlcolorcode, qlstephen, qlmarkdown, quicklook-json, qlprettypatch, quicklook-csv, betterzip, suspicious-package

### Global npm Packages

aicommits, agent-browser

### Global Composer Packages

laravel/envoy, spatie/phpunit-watcher, laravel/valet

### PHP Extensions

imagick, memcached, xdebug, redis

### Agent Skills

vercel-labs/agent-skills (web design, React), anthropics/skills (frontend design), expo/skills (React Native), vercel-labs/agent-browser (browser automation), coreyhaines31/marketingskills (copywriting, SEO), callstackincubator/agent-skills (React Native performance)

## Credits

Created by Freek Van der Herten. Used by many at Spatie.

See Brewfile for a complete list of installed packages.
