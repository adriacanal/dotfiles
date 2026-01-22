# Modern Dotfiles

Personal dotfiles with modern shell tooling, optimized for Laravel/PHP development. Features fast startup times, smart directory navigation, and modern CLI tools.

---

## Quick Start

```bash
git clone git@github.com:freekmurze/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
bin/install
```

After installation, verify everything works:

```bash
bin/doctor    # Health check and diagnostics
```

---

## What's Included

### Shell & Prompt

**Oh My Zsh** - Framework for managing Zsh configuration (with agnoster theme by default)
**Starship** - Fast alternative prompt (optional, 4x faster startup)
**zoxide** - Smart directory jumping based on frecency
**fzf** - Fuzzy finder for files and history
**direnv** - Automatic environment variables per directory

### Modern CLI Tools

**fnm** - Fast Node.js version manager
**bat** - Cat with syntax highlighting
**eza** - Modern ls replacement with icons
**ripgrep** - Fast grep alternative
**fd** - Fast find alternative
**git-delta** - Better git diffs

### Development Tools

**PHP** - Latest version via Homebrew
**Composer** - Dependency manager via Homebrew
**Node.js** - LTS version managed via fnm
**Laravel Valet** - Local development server
**MySQL** - Database with auto-start

### QuickLook Plugins

Instant file previews in Finder: code files, markdown, JSON, CSV, patches, and archives.

---

## How It Works

### Symlinked Files

The installation creates symlinks from your home directory to the dotfiles repository. This allows you to version control your configuration while keeping files in their expected locations.

| Symlink Location | Points To | Purpose |
|-----------------|-----------|---------|
| `~/.zshrc` | `~/.dotfiles/home/.zshrc` | Main Zsh configuration (Oh My Zsh with agnoster theme) |
| `~/.vimrc` | `~/.dotfiles/home/.vimrc` | Vim configuration |
| `~/.vim/` | `~/.dotfiles/home/.vim/` | Vim runtime files |
| `~/.global-gitignore` | `~/.dotfiles/home/.global-gitignore` | Global Git ignore patterns |
| `~/.mackup.cfg` | `~/.dotfiles/macos/.mackup.cfg` | Mackup backup configuration |

### Sourced Files

These files are loaded by `.zshrc` but remain in the dotfiles directory:

- `home/.aliases` - Shell command aliases
- `home/.functions` - Custom shell functions
- `home/.exports` - Environment variables

### Alternative Configuration

A faster alternative using Starship prompt is available at `home/.zshrc.starship` (~50ms startup vs ~200ms for the default Oh My Zsh config). To switch:

```bash
ln -sf ~/.dotfiles/home/.zshrc.starship ~/.zshrc
exec zsh
```

---

## Daily Usage

### Smart Navigation

```bash
z dotfiles          # Jump to frequently used directories
zi                  # Interactive directory picker
Ctrl+R              # Fuzzy search command history
Ctrl+T              # Fuzzy find files
Alt+C               # Fuzzy change directory
```

### Laravel/PHP Shortcuts

```bash
a                   # php artisan
p                   # Run Pest/PHPUnit tests
c                   # composer
mfs                 # php artisan migrate:fresh --seed
nah                 # git reset --hard; git clean -df
```

### Maintenance Commands

```bash
bin/update          # Update all packages and tools
bin/doctor          # Verify installation health
```

---

## Version Management

### Node.js (via fnm)

```bash
fnm install --lts     # Install latest LTS
fnm use lts-latest    # Use latest LTS
fnm install 20        # Install specific version
fnm use 20            # Switch to specific version
fnm list              # Show installed versions
```

### PHP & Composer (via Homebrew)

```bash
brew upgrade php      # Update PHP to latest
brew upgrade composer # Update Composer
```

---

## Package Management

All Homebrew packages are declared in `config/Brewfile`. To add a new tool:

```bash
echo 'brew "neovim"' >> ~/.dotfiles/config/Brewfile
brew bundle --file=~/.dotfiles/config/Brewfile
```

**Complete package list:**

- **Core**: node, php, composer, pkg-config, wget, httpie, ncdu, hub, ack, doctl, 1password-cli, git-secret, imagemagick, mysql, yarn, ghostscript, mackup
- **Modern CLI**: starship, zoxide, bat, eza, ripgrep, fd, git-delta, fnm, fzf, direnv, zsh-autosuggestions
- **QuickLook**: qlcolorcode, qlstephen, qlmarkdown, quicklook-json, qlprettypatch, quicklook-csv, betterzip, suspicious-package
- **PHP Extensions**: imagick, memcached, xdebug, redis
- **Global npm**: aicommits, agent-browser
- **Global Composer**: laravel/envoy, spatie/phpunit-watcher, laravel/valet

---

## Claude Code Integration

### Quick Install (Standalone)

Install just Claude Code without the full dotfiles:

```bash
curl -fsSL https://raw.githubusercontent.com/freekmurze/dotfiles/main/bin/install-claude-code | bash
```

### What Gets Installed

- Claude Code CLI
- Custom configuration (CLAUDE.md, laravel-php-guidelines.md)
- Custom skills (ray-skill, fix-github-issue, convert-issue-to-discussion)
- Community skills (web design, React, React Native, marketing)

### Pre-installed Agent Skills

**Development & Design:**
- `vercel-labs/agent-skills` - Web design guidelines and React best practices
- `anthropics/skills` - Frontend design and skill creation tools
- `vercel-labs/agent-browser` - Browser automation

**Mobile Development:**
- `expo/skills` - React Native with Expo
- `callstackincubator/agent-skills` - React Native performance

**Marketing:**
- `coreyhaines31/marketingskills` - Copywriting and programmatic SEO

Browse more at [skills.sh](https://skills.sh):

```bash
npx skills add <owner/repo>
```

---

## Customization

### Personal Aliases & Functions

Create custom configurations that won't be committed:

```bash
mkdir -p ~/.dotfiles-custom/shell
vim ~/.dotfiles-custom/shell/.aliases
```

These files are automatically loaded by `.zshrc` if they exist.

### Project-Specific Variables

Use `direnv` for automatic environment loading:

```bash
cd my-project
echo 'export DEBUG=true' > .envrc
direnv allow
```

Variables load when you enter the directory and unload when you leave.

---

## Post-Installation

1. **Configure iTerm2 font**:
   - Open iTerm2 Preferences (Cmd+,)
   - Go to Profiles → Text
   - Change Font to **MesloLGM Nerd Font Mono** (size 12-14)
   - Enable "Use built-in Powerline glyphs"

2. **Import theme**: In iTerm2, import `config/iterm/Solarized Dark Corrected.itermcolors`

3. **Restore settings** (optional): Run `mackup restore` if you have backups

4. **Migrate history** (upgrading only): Run `migration/migrate-z-to-zoxide.sh` if you have `~/.z`

---

## Troubleshooting

### Shell Startup is Slow

The default configuration uses Oh My Zsh with agnoster theme (~200ms). For faster startup (~50ms), switch to Starship:

```bash
ln -sf ~/.dotfiles/home/.zshrc.starship ~/.zshrc
exec zsh
```

### Command Not Found

```bash
exec zsh        # Reload shell
bin/doctor      # Check what's missing
```

### Starship Prompt Not Showing

```bash
starship --version
echo $STARSHIP_CONFIG    # Should point to config/starship.toml
```

### fnm Not Switching Versions

```bash
fnm --version
fnm current
fnm list
```

---

## Tool Comparisons

| Old Tool | New Tool | Why Better |
|----------|----------|------------|
| Oh My Zsh themes | Starship | 10x faster, cross-shell compatible |
| z.sh / autojump | zoxide | Smarter frecency algorithm, Rust speed |
| nvm | fnm | 40x faster, simpler, Rust-based |
| cat | bat | Syntax highlighting, git integration |
| ls | eza | Icons, tree view, git status |
| grep | ripgrep | 5-10x faster, respects .gitignore |
| find | fd | Simpler syntax, 10x faster |
| diff | delta | Side-by-side diffs, syntax highlighting |

---

## Utilities

The `bin/` directory contains helper scripts:

- **install** - Main installation script (idempotent, safe to re-run)
- **install-claude-code** - Standalone Claude Code installer
- **update** - Update dotfiles, Homebrew, npm, and Composer packages
- **doctor** - Health check and diagnostic tool

---

## Migration Notes

If upgrading from an older setup:

1. **Directory history**: Run `migration/migrate-z-to-zoxide.sh` to import your `~/.z` data
2. **Prompt**: The default config uses Oh My Zsh with agnoster theme. For a faster alternative, use `home/.zshrc.starship`
3. **Version managers**: fnm replaces nvm, Homebrew manages PHP (no more compilation)

---

## Credits

Created by [Freek Van der Herten](https://github.com/freekmurze). Used by many at [Spatie](https://spatie.be).

See `config/Brewfile` for complete package list.
