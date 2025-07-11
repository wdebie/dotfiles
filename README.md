Just another Sway configuration.

## Installation:
```bash
sudo pacman -S --needed zsh stow 
```

Install [oh-my-zsh](https://ohmyz.sh/), then remove `~/.zshrc`.

```bash
cd ~ && git clone git@github.com:wdebie/dotfiles.git .dotfiles && cd .dotfiles && stow .
```

### Essential additional packages:
Install [yay](https://github.com/Jguer/yay), then run:
```bash
yay -S --needed alacritty fuzzel waybar wofi flameshot cliphist grim xdg-utils swaync zsh polkit polkit-gnome xdg-desktop-portal xdg-desktop-portal-wlr xdg-desktop-portal-gtk playerctl wl-clipboard wl-clipboard-x11 file-roller
```

### Fonts:

```bash
sudo pacman -S $(pacman -Ssq noto-fonts) && sudo pacman -S ttf-font-awesome ttf-jetbrains-mono-nerd
```

**!** *First reload: `Super+Shift+C` (sway default), after which the bind will be `Super+Shift+R`*

### Notes to self:
- Lose your pride and just use `archinstall`.
- I like nemo as a file manager; `nemo nemo-fileroller nemo-terminal`. 
- Essentials: `brave-bin`, `mullvad-bin`, `filen-desktop-bin`.
- Install `flatpak`, get **Warehouse**, add user-repo, delete system-repo, install:
    - Gear Lever
    - Image Viewer
    - Logs
    - Flatseal
    - qBittorrent
    - Spotify
    - Text Editor
    - VLC
    - Obsidian
- Now create Filen syncs for Pictures, Music, Videos, Administrative and Obsidian

### greetd
I like greetd, see [wiki.archlinux.org/title/Greetd](https://wiki.archlinux.org/title/Greetd)  

`/usr/local/bin/start-sway`:
```bash
#!/bin/sh
export XDG_SESSION_TYPE=wayland
export XDG_SESSION_DESKTOP=sway
export XDG_CURRENT_DESKTOP=sway

# Wayland stuff
export QT_QPA_PLATFORM=wayland
export SDL_VIDEODRIVER=wayland
export _JAVA_AWT_WM_NONREPARENTING=1

exec sway "$@"
```

`/etc/greetd/config.toml`:
```bash
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

### Reboot
