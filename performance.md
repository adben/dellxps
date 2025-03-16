# Manjaro Setup - XPS 15 9530 - Quick Notes

## System

* Kernel: 6.12.17-1-MANJARO (x86\_64)
* GNOME 47.4
* Manjaro Linux
* i9-13900H, Intel Iris Xe, RTX 4060 Max-Q
* 64GB RAM, 1TB SSD

## To Do

* **Battery:**
    * TLP: `pacman -S tlp`, enable, config
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

## Done

* **Battery:**
    * TLP:
        * Installed `tlp`
        * Enabled `tlp` service `sudo systemctl enable tlp` and `sudo systemctl start tlp`
        * Configured `/etc/tlp.conf` (see section below for specific settings)
        * Applied changes with `sudo tlp start`
        * Verified status with `sudo tlp-stat -s`
    * Optimus:
        * Installed `optimus-manager` and `optimus-manager-qt`
        * Enabled `optimus-manager` service
        * Tested switching between Intel and NVIDIA GPUs
        * Verified status with `optimus-manager --status`
    * `cpupower`, power settings, services, fstrim:
        * Checked CPU governor with `cpupower frequency-info`
        * Set CPU governor to `powersave` on battery
        * Adjusted GNOME Power settings
        * Reviewed and disabled unnecessary services
        * Enabled `fstrim.timer` service
        * Verified `fstrim` status.

## /etc/tlp.conf Settings

```ini
# /etc/tlp.conf - TLP user configuration

TLP_ENABLE=1
TLP_WARN_LEVEL=3
TLP_MSG_COLORS="91 93 1 92"

# CPU Scaling
CPU_SCALING_GOVERNOR_ON_AC=schedutil
CPU_SCALING_GOVERNOR_ON_BAT=powersave
CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=power
CPU_BOOST_ON_AC=1
CPU_BOOST_ON_BAT=0
PLATFORM_PROFILE_ON_AC=performance
PLATFORM_PROFILE_ON_BAT=low-power

# Disk Power Management
DISK_IDLE_SECS_ON_AC=0
DISK_IDLE_SECS_ON_BAT=2
MAX_LOST_WORK_SECS_ON_AC=15
MAX_LOST_WORK_SECS_ON_BAT=60
DISK_APM_LEVEL_ON_AC="254"
DISK_APM_LEVEL_ON_BAT="128"
DISK_SPINDOWN_TIMEOUT_ON_AC="0"
DISK_SPINDOWN_TIMEOUT_ON_BAT="0"
DISK_IOSCHED="mq-deadline"
SATA_LINKPWR_ON_AC="med_power_with_dipm"
SATA_LINKPWR_ON_BAT="med_power_with_dipm"
AHCI_RUNTIME_PM_ON_AC=on
AHCI_RUNTIME_PM_ON_BAT=auto
AHCI_RUNTIME_PM_TIMEOUT=15

# Intel GPU (if needed, adjust frequencies)
INTEL_GPU_MIN_FREQ_ON_AC=0
INTEL_GPU_MIN_FREQ_ON_BAT=0
INTEL_GPU_MAX_FREQ_ON_AC=0
INTEL_GPU_MAX_FREQ_ON_BAT=0
INTEL_GPU_BOOST_FREQ_ON_AC=0
INTEL_GPU_BOOST_FREQ_ON_BAT=0

# Wi-Fi Power Saving
WIFI_PWR_ON_AC=off
WIFI_PWR_ON_BAT=on

# Disable Wake-on-LAN
WOL_DISABLE=Y

# Audio Power Saving
SOUND_POWER_SAVE_ON_AC=1
SOUND_POWER_SAVE_ON_BAT=1
SOUND_POWER_SAVE_CONTROLLER=Y

# PCIe ASPM
PCIE_ASPM_ON_AC=default
PCIE_ASPM_ON_BAT=powersave

# Runtime Power Management
RUNTIME_PM_ON_AC=on
RUNTIME_PM_ON_BAT=auto
RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon xhci_hcd"

# USB Autosuspend
USB_AUTOSUSPEND=1
USB_EXCLUDE_AUDIO=1
USB_EXCLUDE_PRINTER=1

# Battery Charge Thresholds (If Applicable - Check with tlp-stat -b)
#START_CHARGE_THRESH_BAT0=75
#STOP_CHARGE_THRESH_BAT0=80

# Radio Device Wizard (If Needed)
#DEVICES_TO_DISABLE_ON_LAN_CONNECT="wifi wwan"
#DEVICES_TO_DISABLE_ON_WIFI_CONNECT="wwan"
#DEVICES_TO_DISABLE_ON_WWAN_CONNECT="wifi"
#DEVICES_TO_ENABLE_ON_LAN_DISCONNECT="wifi wwan"
#DEVICES_TO_ENABLE_ON_WIFI_DISCONNECT=""
#DEVICES_TO_ENABLE_ON_WWAN_DISCONNECT=""
#DEVICES_TO_ENABLE_ON_DOCK=""
#DEVICES_TO_DISABLE_ON_DOCK=""
#DEVICES_TO_ENABLE_ON_UNDOCK="wifi"
#DEVICES_TO_DISABLE_ON_UNDOCK=""
```
Explanation of Settings:

    CPU Scaling:
        schedutil on AC: A good balance of performance and power saving.
        powersave on BAT: Aggressive power saving.
        balance_performance / power: Sets the energy/performance policy for better power efficiency.
        Boost is disabled on battery to save power.
    Disk Power Management:
        APM levels are set for moderate power saving.
        Spindown is disabled, as it can be counterproductive on SSDs.
        mq-deadline is a good I/O scheduler for SSDs.
        AHCI runtime PM is enabled on battery.
    Wi-Fi:
        Wi-Fi power saving is enabled on battery.
    PCIe ASPM:
        powersave on battery enables PCIe power saving.
    Runtime PM:
        Enabled on battery.
    USB Autosuspend:
        Enabled for most USB devices, excluding audio and printers.
    Battery Thresholds:
        You'll need to check if your Dell XPS 15 9530 supports battery charge thresholds using sudo tlp-stat -b. If it does, uncomment and adjust the thresholds as needed.

Important Notes:

    After making changes, save the file and run sudo tlp start to apply them.
    Monitor your battery life and performance to fine-tune the settings.
    The Intel GPU frequency settings are left at default for now. Adjust them if needed.
    Always test after changes. for instance :::

```bash
        ~/p/dellxps    main !1  tlp-stat -p                                     ✔  15s 
--- TLP 1.8.0 --------------------------------------------

+++ Processor
CPU model = 13th Gen Intel(R) Core(TM) i9-13900H

/sys/devices/system/cpu/cpu0/cpufreq/scaling_driver    = intel_pstate
/sys/devices/system/cpu/cpu0/cpufreq/scaling_governor  = powersave
/sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors = performance powersave
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq  =   400000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq  =  5200000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_min_freq  =   400000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq  =  5200000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/energy_performance_preference = balance_performance [EPP]
/sys/devices/system/cpu/cpu0/cpufreq/energy_performance_available_preferences = default performance balance_performance balance_power power

/sys/devices/system/cpu/cpu1..cpu19: omitted for clarity, use -v to show all

/sys/devices/system/cpu/intel_pstate/status            = active
/sys/devices/system/cpu/intel_pstate/min_perf_pct      =   8 [%]
/sys/devices/system/cpu/intel_pstate/max_perf_pct      = 100 [%]
/sys/devices/system/cpu/intel_pstate/no_turbo          =   0
/sys/devices/system/cpu/intel_pstate/hwp_dynamic_boost =   0
/sys/devices/system/cpu/intel_pstate/turbo_pct         = (not available)
/sys/devices/system/cpu/intel_pstate/num_pstates       = (not available)
/sys/module/workqueue/parameters/power_efficient       = Y
/proc/sys/kernel/nmi_watchdog                          = 0

+++ Platform Profile
/sys/firmware/acpi/platform_profile                    = performance
/sys/firmware/acpi/platform_profile_choices            = cool quiet balanced performance
```

    this was after

```
Error in configuration at CPU_SCALING_GOVERNOR_ON_AC="schedutil": governor not available.
Error in configuration at INTEL_GPU_MIN_FREQ_ON_AC="0": frequency invalid or out of range (see 'tlp-stat -g').
```

as result of it


```ini
# /etc/tlp.conf - TLP user configuration

TLP_ENABLE=1
TLP_WARN_LEVEL=3
TLP_MSG_COLORS="91 93 1 92"

# CPU Scaling
CPU_SCALING_GOVERNOR_ON_AC=performance # Use performance on AC
CPU_SCALING_GOVERNOR_ON_BAT=powersave
CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=power
CPU_BOOST_ON_AC=1
CPU_BOOST_ON_BAT=0
PLATFORM_PROFILE_ON_AC=performance
PLATFORM_PROFILE_ON_BAT=balanced # balanced for better battery.

# Disk Power Management
DISK_IDLE_SECS_ON_AC=0
DISK_IDLE_SECS_ON_BAT=2
MAX_LOST_WORK_SECS_ON_AC=15
MAX_LOST_WORK_SECS_ON_BAT=60
DISK_APM_LEVEL_ON_AC="254"
DISK_APM_LEVEL_ON_BAT="128"
DISK_SPINDOWN_TIMEOUT_ON_AC="0"
DISK_SPINDOWN_TIMEOUT_ON_BAT="0"
DISK_IOSCHED="mq-deadline"
SATA_LINKPWR_ON_AC="med_power_with_dipm"
SATA_LINKPWR_ON_BAT="med_power_with_dipm"
AHCI_RUNTIME_PM_ON_AC=on
AHCI_RUNTIME_PM_ON_BAT=auto
AHCI_RUNTIME_PM_TIMEOUT=15

# Intel GPU (Removed or Commented Out)
#INTEL_GPU_MIN_FREQ_ON_AC=0
#INTEL_GPU_MIN_FREQ_ON_BAT=0
#INTEL_GPU_MAX_FREQ_ON_AC=0
#INTEL_GPU_MAX_FREQ_ON_BAT=0
#INTEL_GPU_BOOST_FREQ_ON_AC=0
#INTEL_GPU_BOOST_FREQ_ON_BAT=0

# Wi-Fi Power Saving
WIFI_PWR_ON_AC=off
WIFI_PWR_ON_BAT=on

# Disable Wake-on-LAN
WOL_DISABLE=Y

# Audio Power Saving
SOUND_POWER_SAVE_ON_AC=1
SOUND_POWER_SAVE_ON_BAT=1
SOUND_POWER_SAVE_CONTROLLER=Y

# PCIe ASPM
PCIE_ASPM_ON_AC=default
PCIE_ASPM_ON_BAT=powersave

# Runtime Power Management
RUNTIME_PM_ON_AC=on
RUNTIME_PM_ON_BAT=auto
RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon xhci_hcd"

# USB Autosuspend
USB_AUTOSUSPEND=1
USB_EXCLUDE_AUDIO=1
USB_EXCLUDE_PRINTER=1

# Battery Charge Thresholds (If Applicable - Check with tlp-stat -b)
#START_CHARGE_THRESH_BAT0=75
#STOP_CHARGE_THRESH_BAT0=80

# Radio Device Wizard (If Needed)
#DEVICES_TO_DISABLE_ON_LAN_CONNECT="wifi wwan"
#DEVICES_TO_DISABLE_ON_WIFI_CONNECT="wwan"
#DEVICES_TO_DISABLE_ON_WWAN_CONNECT="wifi"
#DEVICES_TO_ENABLE_ON_LAN_DISCONNECT="wifi wwan"
#DEVICES_TO_ENABLE_ON_WIFI_DISCONNECT=""
#DEVICES_TO_ENABLE_ON_WWAN_DISCONNECT=""
#DEVICES_TO_ENABLE_ON_DOCK=""
#DEVICES_TO_DISABLE_ON_DOCK=""
#DEVICES_TO_ENABLE_ON_UNDOCK="wifi"
#DEVICES_TO_DISABLE_ON_UNDOCK=""
```
then
```bash
     sudo tlp start                               ✔  2m 42s 
TLP started in AC mode (auto).
```
