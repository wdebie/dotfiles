# Dotfiles
Config files for my personal Arch Linux configuration. I’m currently using both Xfce and i3-wm desktop environments. However, the configuration files for each are so small that their impact on storage space is negligible. In the final step of [Installation](#installation) you’ll find packages for both environments. If you prefer to use only one, feel free to review the file(s) and select the relevant packages. 🐧🔧

## Installation
#### Quick install to device
```sh
sudo pacman -S chezmoi
```
```sh
chezmoi init --apply --ssh wdebie
```
```sh
sudo pacman -S --needed - < ~/.pkg_list/cleanpkglist_noforeign ;
yay -S - < ~/.pkg_list/foreignpkglist
```
#### Work with chezmoi: [read the docs](https://www.chezmoi.io/user-guide/command-overview/) and check out [#automation](#automation). 

## Configuration

#### 1. Git user information and GPG signingkey
Just so you don't sign commits with my email and username (even though it doesn't matter because I sign all of my commits), let's change them:
```config
git config --global user.name user1
git config --global user.email user1@example.com
```

Note that the 'signingkey' in `.gitconfig` will be wrong when you import these dotfiles because that's my personal key that I use to sign my commits.
To change this to your own GPG key ID, run the following command:
```config
git config --global user.signingkey <key_id>
```

To remove it entirely, run:
```config
git config --global --unset user.signingkey
```


#### 2. Setting wallpaper
To set wallpapers on i3 startup, install [nitrogen](https://wiki.archlinux.org/title/nitrogen) and uncomment the following line in `~/.config/i3/config`:
```config
#exec --no-startup-id nitrogen --set-zoom-fill /path/to/image.png.svg.jpg
```
This will override the nitrogen wallpaper that is currently used in the config file. By uncommenting this line instead of editing the original command it will always go back to the default wallpaper when running into problems. For the sake of testing, use `exec_always`<sup>*</sup> instead of `exec` to view changes on [i3 restart](#keybinds).

\* [#_automatically_starting_applications_on_i3_startup](https://i3wm.org/docs/userguide.html#_automatically_starting_applications_on_i3_startup)

#### 3. Sup-mail scratchpad
Uncomment the following line in `~/.config/i3/config` to show the sup-mail scratchpad window, if any exist.
```config
#bindsym mod4+s [title="^Sup ::"] scratchpad show
```

#### 4. Other keybinds to focus/move windows
The default keys to change focus or move windows in i3 are `j`, `k`, `l` and `;`. I disabled these in my config since arrow keys are more convenient for me. To re-bind these take a look at the default i3 config file at `/etc/i3/config` and copy the required lines to `~/.config/i3/config`.


#### 5. GRUB theme
This is obviously not required, but I use [this theme](https://github.com/AdisonCavani/distro-grub-themes/releases/download/v3.1/arch.tar) for my GRUB installation. Also look at the [grub-customizer](https://archlinux.org/packages/community/x86_64/grub-customizer/) package to install this easily.


#### 6. /etc/ files
Files that start with "etc_" in this repository can be savely moved to `/etc/`, they're simple configurations that I wanted to add to the dotfiles. Look them through, they're unrelated to the other config files and thus optional.

#### 7. Scripts
To add a selection from `~/Scripts` to `/usr/local/bin`, I recommend using my "create-symlinks" script. Make sure to run it from the same user that you've initialized chezmoi with.

```sh
sudo sh /home/$USER/Scripts/create-symlinks.sh
```

#### 8. Default applications
The [XDG MIME Applications specification](https://specifications.freedesktop.org/mime-apps-spec/mime-apps-spec-latest.html) builds upon the [shared MIME database](https://wiki.archlinux.org/title/XDG_MIME_Applications#Shared_MIME_database) and [desktop entries](https://wiki.archlinux.org/title/Desktop_entries) to provide [default applications](https://wiki.archlinux.org/title/Default_applications). ([source](https://wiki.archlinux.org/title/XDG_MIME_Applications))

While it is possible to configure default applications by directly editing [mimeapps.list](https://wiki.archlinux.org/title/XDG_MIME_Applications#mimeapps.list), I recommend using a GUI tool to simplify this process.

- [selectdefaultapplication-git](https://aur.archlinux.org/packages/selectdefaultapplication-git)<sup>aur</sup>

#### 9. Qt5 toolkit styling
To make my desktop experience better, I try to work exclusively with Qt-based applications whenever I need a GUI tool. [Read the wiki.](https://wiki.archlinux.org/title/qt#Styles_in_Qt_5) Additionally, to do proper styling for all Qt applications, read [qt#Configuration_of_Qt_5_applications_under_environments_other_than_KDE_Plasma](https://wiki.archlinux.org/title/qt#Configuration_of_Qt_5_applications_under_environments_other_than_KDE_Plasma)


#### 10. Randomized lightdm-slick-greeter wallpaper
Check out [RANDOM_LIGHTDM_BG.md](RANDOM_LIGHTDM_BG.md).


## Keybinds
|Keybind|Action|Keybind|Action|
|:---|:---|:---|:---|
|$mod + Enter|kitty (terminal-emulator)|$mod + a|focus parent|
|$mod + Shift + q|kill window|$mod + s|stacked layout|
|$mod + d|rofi dmenu|$mod + $number|workspace number $number|
|$mod + prt sc|[flameshot](https://archlinux.org/packages/community/x86_64/flameshot/) gui|$mod + Shift + $number|move container to workspace $number
|$mod + left arrow|focus left|$mod + v|open [greenclip<sup>aur</sup>](https://aur.archlinux.org/packages/rofi-greenclip)|
|$mod + down arrow|focus down|$mod + Shift + minus|make [scratchpad](https://i3wm.org/docs/userguide.html#_scratchpad) of current window|
|$mod + up arrow|focus up|$mod + minus|scratchpad toggle|
|$mod + right arrow|focus right|$mod + Shift + c|reload config file|
|$mod + f|fullscreen toggle|$mod + Shift + e|exit i3|
|$mod + w|layout tabbed layout|$mod + Shift + r|restart i3|
|$mod + e|layout toggle split|$mod + h|split horizontally|
|$mod + Shift + Space|floating toggle|$mod + b|split vertically|
|$mod + Space|focus mode_toggle|$mod + Shift + arrows|move window around|

*\* My personal $mod is Mod4 (Super).*

## Automation
This is a simple script I wrote to automate chezmoi edits:
> ~/.config/chezmoi/automation.sh
```sh
#!/bin/bash
if [[ $EUID -eq 0 ]]; then
    echo "This script cannot be run as root."
    exit 1
fi
cd ~/.local/share/chezmoi
chezmoi re-add
read -p "Commit message: " MESSAGE
git commit -a -S -m "$MESSAGE"
git push --set-upstream origin main
```
All you have to do is change a dotfile (e.g. `~/.config/polybar/config`) and when you're done run this script. This script does not handle new files, only edits to files you've already added to chezmoi. This script cannot be run as root. If you don't want to sign your commits, remove `-S` from the commit command.

There's a fork of this in `~/Scripts` with some extra lines added to keep a list of installed pacman packages for this dotfiles repository, it's there if you need it but in most cases it is redundant. More details: [Pacman/Tips_and_tricks#List_of_installed_packages](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks#List_of_installed_packages).

## Greenclip
I didn't include my greenclip configuration in the dotfiles because I have some sensitive information stored as [static history](https://github.com/erebe/greenclip#description). However, if you want a similar outlook, here's the config that I use:

> ~/.config/greenclip.toml
```toml
[greenclip]
  blacklisted_applications = []
  enable_image_support = true
  history_file = "/home/[USER]/.cache/greenclip.history"
  image_cache_directory = "/tmp/greenclip"
  max_history_length = 10
  max_selection_size_bytes = 0
  static_history = [" ", "--'~/.config/greenclip.toml'--", "[PERMANENTLY STORED INFO HERE]"]
  trim_space_from_selection = true
  use_primary_selection_as_input = false
```

<hr/>


- Credit to [@Eydou](https://github.com/Eydou) for the `.bashrc` PS1 prompt.
