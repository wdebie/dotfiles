Just another Sway configuration.

## Installation:
```bash
sudo pacman -S --needed zsh stow 
```

Install [oh-my-zsh](https://ohmyz.sh/), then remove `~/.zshrc`.

```
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

### Reboot
