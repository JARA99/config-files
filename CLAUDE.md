# Claude Instructions for Dotfiles Management

This repository contains my personal dotfiles managed with GNU Stow.

## Repository Structure

- `.config/nvim/` - Neovim configuration (git submodule from https://github.com/JARA99/nvim-config)
- `.config/tmux/tmux.conf` - Tmux configuration with:
  - Seamless nvim-tmux navigation using nvim-tmux-navigation plugin
  - Vi-style copy mode keybindings
  - Clipboard integration with system clipboard
  - True color support

## How Stow Works

GNU Stow creates symlinks from this directory to the home directory, mirroring the structure:
- `~/.dotfiles/.config/nvim` → symlinked to `~/.config/nvim`
- `~/.dotfiles/.config/tmux` → symlinked to `~/.config/tmux`

## Important Commands

### Applying dotfiles (creating symlinks)
```bash
cd ~/.dotfiles
stow .
```

### Removing symlinks
```bash
cd ~/.dotfiles
stow -D .
```

### Updating the nvim submodule
```bash
cd ~/.dotfiles
git submodule update --remote .config/nvim
git add .config/nvim
git commit -m "Update nvim submodule"
```

## When Adding New Configurations

1. **Add the config file to ~/.dotfiles** maintaining the home directory structure:
   ```bash
   # Example: Adding .bashrc
   cp ~/.bashrc ~/.dotfiles/.bashrc
   ```

2. **Backup the original**:
   ```bash
   mv ~/.bashrc ~/.bashrc.bak
   ```

3. **Re-stow to create symlinks**:
   ```bash
   cd ~/.dotfiles
   stow --restow .
   ```

4. **Commit to git**:
   ```bash
   git add .
   git commit -m "Add bashrc configuration"
   ```

## When Modifying Configurations

Since the files are symlinked, editing `~/.config/tmux/tmux.conf` actually edits `~/.dotfiles/.config/tmux/tmux.conf`. After making changes:

```bash
cd ~/.dotfiles
git add .config/tmux/tmux.conf
git commit -m "Update tmux configuration: <description>"
git push
```

## Nvim Configuration (Submodule)

The nvim configuration is a separate git repository. To make changes:

1. Navigate to the submodule:
   ```bash
   cd ~/.dotfiles/.config/nvim
   ```

2. Make your changes and commit:
   ```bash
   git add .
   git commit -m "Description of changes"
   git push
   ```

3. Update the parent repository to track the new commit:
   ```bash
   cd ~/.dotfiles
   git add .config/nvim
   git commit -m "Update nvim submodule"
   git push
   ```

## Current Configurations

### Nvim
- Based on kickstart.nvim
- Custom plugins in `lua/custom/plugins/init.lua`:
  - nvim-tmux-navigation for seamless tmux integration
- Custom keymaps:
  - `kj` in insert mode → Esc
  - `lkj` in terminal mode → Normal mode
- Clipboard synced with system (`unnamedplus`)

### Tmux
- Mouse support enabled
- Vi-mode keys
- True color support for nvim
- Seamless navigation with nvim using Ctrl+hjkl
- Vi-style copy mode: `v` to select, `y` to yank
- Clipboard integration enabled

## Backups

Original configurations are backed up as:
- `~/.tmux.conf.bak`
- `~/.config/nvim.bak`

## Troubleshooting

### Symlinks broken after moving files
```bash
cd ~/.dotfiles
stow --restow .
```

### Submodule not initialized after fresh clone
```bash
cd ~/.dotfiles
git submodule init
git submodule update
```

### Conflicts when stowing
```bash
# Remove conflicting files/symlinks first
rm ~/.config/tmux
cd ~/.dotfiles
stow .
```
