# wdebie/dotfiles

My personal config files and package picks for Sway on Arch Linux. Just sharing these in case they're useful to anyone else - not intended to be a plug-and-play solution, more of a 'take what you need' kinda thing.


## Prerequisites

Install the basic required packages:

```bash
sudo pacman -S --needed zsh stow 
```

Install [oh-my-zsh](https://ohmyz.sh/) and remove the default configuration:

```bash
rm ~/.zshrc
```

## Installation

Clone and apply the dotfiles configuration:

```bash
cd ~ && git clone git@github.com:wdebie/dotfiles.git .dotfiles && cd .dotfiles && stow .
```

## Packages

First, install [yay](https://github.com/Jguer/yay), then install pkgs:

```bash
yay -S --needed \
    alacritty \
    fuzzel \
    waybar \
    wofi \
    flameshot \
    cliphist \
    grim \
    xdg-utils \
    swaync \
    zsh \
    polkit \
    polkit-gnome \
    xdg-desktop-portal \
    xdg-desktop-portal-wlr \
    xdg-desktop-portal-gtk \
    playerctl \
    wl-clipboard \
    wl-clipboard-x11 \
    file-roller
```

## Fonts

```bash
sudo pacman -S $(pacman -Ssq noto-fonts) && sudo pacman -S ttf-font-awesome ttf-jetbrains-mono-nerd
```

## Configuration Notes

### First Launch

After installation, reload the configuration:
- **First reload**: `Super+Shift+C` (Sway default)
- **Subsequent reloads**: `Super+Shift+R` (custom binding)

## Display Manager Setup

I like `greetd` as a lightweight display manager with autologin, which is convenient to use with full-disk encryption. See [Arch Linux Greetd Wiki](https://wiki.archlinux.org/title/Greetd).

### Config files

Create the Sway startup script at `/usr/local/bin/start-sway`:

```bash
#!/bin/sh
export XDG_SESSION_TYPE=wayland
export XDG_SESSION_DESKTOP=sway
export XDG_CURRENT_DESKTOP=sway

# Wayland environment variables
export QT_QPA_PLATFORM=wayland
export SDL_VIDEODRIVER=wayland
export _JAVA_AWT_WM_NONREPARENTING=1

exec sway "$@"
```

Configure greetd at `/etc/greetd/config.toml`:

```toml
[terminal]
# The VT to run the greeter on. Can be "next", "current" or a number
# designating the VT.
vt = 1

[initial_session]
command = "/usr/local/bin/start-sway"
user = "wdb"

# The default session, also known as the greeter.
[default_session]

# `agreety` is the bundled agetty/login-lookalike. You can replace `/bin/sh`
# with whatever you want started, such as `sway`.
command = "agreety --cmd /usr/local/bin/start-sway"

# The user to run the command as. The privileges this user must have depends
# on the greeter. A graphical greeter may for example require the user to be
# in the `video` group.
user = "greeter"
```

## Notes to self

- Essentials: `brave-bin mullvad-bin filen-desktop-bin`
- I like nemo as a file manager: `nemo nemo-fileroller nemo-terminal`
- Install flatpak, get Warehouse, add user-repo, remove system-repo.

Daily use flatpak applications:
- Gear Lever
- Image Viewer
- Logs
- Flatseal
- qBittorrent
- Spotify
- Text Editor
- VLC
- Obsidian

After installing Filen, create syncs for:
- Pictures
- Music  
- Videos
- Administrative
- Obsidian
