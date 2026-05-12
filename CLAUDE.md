# Claude Instructions for Dotfiles Management

This repository contains my personal dotfiles managed with GNU Stow.

## IMPORTANT: AI Assistant Limitations

**Claude CANNOT perform git operations that require SSH authentication.**

- **DO NOT** attempt to `git push` or `git pull` on repositories with SSH remotes
- The dotfiles remote uses SSH: `git@github.com:JARA99/config-files.git`
- **INSTEAD**: Inform the user and ask them to run the git commands themselves
- You CAN: make commits, stage files, and prepare changes for the user to push
- You CAN: read git status, logs, and diffs

When the user requests changes that need to be pushed:
1. Make the changes and commit them locally
2. Tell the user: "I've committed the changes. Please run `git push` to push to the remote."
3. Provide the exact commands they need to run

## Repository Structure

- `.config/nvim/` - Neovim configuration (kickstart.nvim base, custom tweaks under `lua/custom/`)
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

### Conflicts when stowing
```bash
# Remove conflicting files/symlinks first
rm ~/.config/tmux
cd ~/.dotfiles
stow .
```
