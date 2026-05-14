# Dotfiles

My personal dotfiles managed with GNU Stow.

## Structure

- `.config/nvim/` - Neovim configuration (submodule from [JARA99/nvim-config](https://github.com/JARA99/nvim-config))
- `.config/tmux/tmux.conf` - Tmux configuration
- `.ssh/rc` - SSH connection hook (see [SSH agent forwarding](#ssh-agent-forwarding))

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

4. Add the SSH agent forwarding shell-init line — see [SSH agent forwarding](#ssh-agent-forwarding) below. Skipping this step is fine if you do not use SSH agent forwarding through long-lived tmux sessions.

## SSH agent forwarding

The repo includes two of the three pieces needed to make SSH agent forwarding survive long-lived tmux sessions across reconnects:

- `.ssh/rc` — runs on every incoming SSH connection; updates `~/.ssh/ssh_auth_sock` to point at whatever socket the new connection forwarded.
- `.config/tmux/tmux.conf` — sets `SSH_AUTH_SOCK $HOME/.ssh/ssh_auth_sock` for new tmux servers, so panes use the stable symlink instead of the raw forwarded path.

The third piece is **not stowed**, because shell rc files (`~/.bashrc`, `~/.zshrc`, `~/.config/fish/config.fish`) vary too much per user and per shell to ship a one-size-fits-all version. On a new install, add this line to your interactive shell init:

```bash
[ -S "$HOME/.ssh/ssh_auth_sock" ] && export SSH_AUTH_SOCK="$HOME/.ssh/ssh_auth_sock"
```

Why all three are needed: `.ssh/rc` keeps the symlink fresh, the tmux conf line applies to *new* tmux servers, and the shell init line covers shells running inside a tmux server that was started before this setup landed (and also direct SSH sessions outside tmux). Skipping the shell line means `ssh-add -l` works in a fresh login shell but fails inside long-lived tmux panes — `git push` from there fails with `Permission denied (publickey)`.

Diagnostic: if `ssh-add -l` fails inside a tmux pane but works in a fresh SSH session, that shell-init line is the missing piece.

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
