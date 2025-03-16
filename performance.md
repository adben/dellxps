# Manjaro Setup - XPS 15 9530 - Quick Notes

## System

* Kernel: 6.12.17-1-MANJARO (x86\_64)
* GNOME 47.4
* Manjaro Linux
* i9-13900H, Intel Iris Xe, RTX 4060 Max-Q
* 64GB RAM, 1TB SSD

## To Do

* **Battery:**
    * TLP: `pacman -S tlp tlp-ui`, enable, config
    * Optimus: `pacman -S optimus-manager optimus-manager-qt`, intel/nvidia switch
    * `cpupower`, power settings, services, fstrim
* **Looks:**
    * GNOME extensions: dash-to-panel, arcmenu, blur, just perfection, maybe material shell
    * Themes/icons: GNOME Look, AUR
    * Ghostty: powerline, colors, zsh plugins
    * Wallpapers
* **Dev:**
    * Podman: `pacman -S podman`, rootless?
    * Tools: `pacman -S git vim tmux neovim htop exa bat ripgrep fd-find`
    * SDKMAN: check install, path
    * Zsh plugins: autosuggest, syntax-highlighting
    * Virtualization: virtualbox/kvm
* **Maintain:**
    * Topgrade
    * Timeshift: `pacman -S timeshift`, setup
    * `yay`
    * NVIDIA drivers
* **Ghostty/Zsh:**
    * `.zshrc`: cleanup, sdkman, aliases
    * Ghostty: appearance, keybindings, tmux

## Ghostty/Zsh quick config notes

* Ghostty themes: Look into Dracula, solarized.
* Zsh plugins:
    * `zsh-autosuggestions`
    * `zsh-syntax-highlighting`
    * `zsh-completions`
    * `zsh-history-substring-search`
* tmux config:
    * Prefix key change to ctrl+a.
    * Plugin manager (tpm).
    * Themes, and status bar.
    * Keybindings for window and pane management.
* Powerline fonts: Fira code nerd font.
* Aliases: review and trim, add specific aliases for k3s and podman.
* Sdkman troubleshooting:
    * Check .sdkman/bin is in path.
    * Reinstall if needed.

## Podman/Docker Notes

* Podman alias to docker.
* Podman compose.
* Rootless podman setup.
* k3s integration with podman.
* Check for existing docker containers, and migrate them to podman.