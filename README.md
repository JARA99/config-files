# Dotfiles

My personal dotfiles managed with GNU Stow.

## Structure

- `.config/nvim/` - Neovim configuration (submodule from [JARA99/nvim-config](https://github.com/JARA99/nvim-config))
- `.config/tmux/tmux.conf` - Tmux configuration

## Installation

1. Clone this repository:
   ```bash
   git clone <your-repo-url> ~/.dotfiles
   cd ~/.dotfiles
   ```

2. Initialize and update submodules:
   ```bash
   git submodule init
   git submodule update
   ```

3. Use GNU Stow to create symlinks:
   ```bash
   cd ~/.dotfiles
   stow .
   ```

## Updating

To update the nvim configuration:
```bash
cd ~/.dotfiles/.config/nvim
git pull
cd ~/.dotfiles
git add .config/nvim
git commit -m "Update nvim submodule"
```

## Removing

To remove the symlinks:
```bash
cd ~/.dotfiles
stow -D .
```
