# Manjaro Setup - XPS 15 9530 - Quick Notes

## System

* Kernel: 6.12.17-1-MANJARO (x86\_64)
* GNOME 47.4
* Manjaro Linux
* i9-13900H, Intel Iris Xe, RTX 4060 Max-Q
* 64GB RAM, 1TB SSD

## See done section for completed tasks
* **Battery:**
    * TLP: `pacman -S tlp`, enable, config
    * Optimus: `pacman -S optimus-manager optimus-manager-qt`, intel/nvidia switch
    * `cpupower`, power settings, services, fstrim
* **Maintain:**
    * Topgrade
    * Timeshift: `pacman -S timeshift`, setup
    * `yay`
    * NVIDIA drivers

* **Ghostty/Zsh:**
    * `.zshrc`: cleanup, sdkman, aliases
    * Ghostty: appearance, keybindings, tmux

## To Do
* **Dev:**
    * Podman: `pacman -S podman`, rootless?
    * Tools: `pacman -S git vim tmux neovim htop exa bat ripgrep fd-find`
    * SDKMAN: check install, path
    * Zsh plugins: autosuggest, syntax-highlighting
    * Virtualization: virtualbox/kvm



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
see the tlp.conf file for more details and final configuration. then...

1. **Apply this configuration:**
   ```bash
   sudo cp /path/to/this/config /etc/tlp.conf
   sudo tlp start
   ```

2. **Create system optimization file:**
   ```bash
   sudo tee /etc/sysctl.d/99-performance.conf << EOF
   # I/O and memory management optimizations
   vm.dirty_ratio = 10
   vm.dirty_background_ratio = 5
   vm.vfs_cache_pressure = 50

   # Network performance
   net.core.somaxconn = 4096
   net.ipv4.tcp_max_syn_backlog = 4096
   net.ipv4.tcp_slow_start_after_idle = 0

   # Hybrid CPU scheduler optimization
   kernel.sched_migration_cost_ns = 5000000
   EOF

   sudo sysctl --system
   ```

~~3. **Add noatime to root filesystem in /etc/fstab:**
    ```
    UUID=9528931f-ab51-46ed-b8b3-d00e691d14ea / ext4 defaults,noatime 0 1
    ```~~

4. **Configure NVMe scheduler:**
   ```bash
   echo 'ACTION=="add|change", KERNEL=="nvme[0-9]n[0-9]", ATTR{queue/scheduler}="none"' | sudo tee /etc/udev/rules.d/60-scheduler.rules
   ```

These settings will provide optimal performance when plugged in while maximizing battery life when unplugged, properly leveraging the i9-13900H's hybrid architecture and managing the dual GPUs effectively.

### 2. NVIDIA/Intel GPU Management (High Impact)
Your `optimus-manager` setup requires manual switching between GPUs.

```bash
# Install auto-switching tool
sudo pacman -S envycontrol

# Set hybrid mode with on-demand NVIDIA activation
sudo envycontrol --switch hybrid

# Create a script to run specific apps with NVIDIA
cat > ~/.local/bin/nvidia-run << 'EOF'
#!/bin/bash
__NV_PRIME_RENDER_OFFLOAD=1 __VK_LAYER_NV_optimus=NVIDIA_only __GLX_VENDOR_LIBRARY_NAME=nvidia "$@"
EOF
chmod +x ~/.local/bin/nvidia-run
```

**Benefit**: 2-3 hour battery life improvement while maintaining GPU availability.

# SSD TRIM (fstrim) Configuration

**Description:**

This section details how to enable and verify the `fstrim` service, which is essential for maintaining SSD performance and longevity.

**Steps:**

1.  **Enable `fstrim.timer`:**
    * Open a terminal.
    * Run the following command to enable the `fstrim.timer` service:
        ```bash
        sudo systemctl enable fstrim.timer
        ```
    * This command configures the system to automatically run `fstrim` weekly.

2.  **Verify `fstrim.timer` Status:**
    * To check the status of the `fstrim.timer` service, run:
        ```bash
        systemctl status fstrim.timer
        ```
    * This command will display information about the service, including its current status, when it will run next, and any related logs.
    * Example output:
        ```
        ● fstrim.timer - Discard unused filesystem blocks once a week
            Loaded: loaded (/usr/lib/systemd/system/fstrim.timer; enabled; preset: disabled)
            Active: active (waiting) since Sun 2025-03-16 12:36:41 CET; 7min ago
        Invocation: 22ca00219db94f8caa42fb29cf65c85f
            Trigger: Mon 2025-03-17 00:17:37 CET; 11h left
        Triggers: ● fstrim.service
            Docs: man:fstrim

        mrt 16 12:36:41 adolfo-xps159530 systemd[1]: Started Discard unused filesystem blocks once a week.
        ```
    * The "Active: active (waiting)" status indicates that the timer is enabled and waiting for its scheduled execution.

3.  **Manual `fstrim` Execution (Optional):**
    * You can manually run `fstrim` to immediately discard unused blocks.
    * Run:
        ```bash
        sudo fstrim -av
        ```
    * This command will TRIM all mounted filesystems that support the operation.
    * Example output:
        ```
        /boot/efi: 298,9 MiB (313466880 bytes) trimmed on /dev/nvme0n1p1
        fstrim: /: the discard operation is not supported
        ```

**Troubleshooting:**

* **"fstrim: /: the discard operation is not supported" Error:**
    * This error message indicates that the root filesystem (`/`) does not support the discard operation.
    * This typically occurs when the filesystem is not mounted with the `discard` option or when the underlying storage does not support TRIM.
    * In most cases, this error can be safely ignored, as the `/boot/efi` partition (and any other supported partitions) will still be trimmed.
    * If you are using BTRFS, it is very common that trim is not supported.
    * If you wish to enable discard on the root filesystem, you will need to edit your `/etc/fstab` file, and add the discard option to the mount options. This is not recommended, as it can cause performance issues.

**Important Notes:**

* `fstrim` is essential for maintaining SSD performance.
* The `fstrim.timer` service automates the TRIM process.
* The error message "discard operation is not supported" on the root file system is common and often not a cause for concern.
* If you are using BTRFS, then it is very common that you will get the "discard operation is not supported" message.

# Eyecandy Customization

how I've enhanced the visual appearance of my GNOME desktop:

## 1. Caffeine (GNOME Extension)

* **Purpose:**
    * I use Caffeine to prevent my screen from dimming or locking during presentations or video watching.
* **Installation:**
    * Installed the GNOME Extensions
    * Installed the Caffeine extension through the Extensions app.
* **Usage:**
    * Toggle Caffeine on/off via the system tray icon.

## 2. Selected Nerd Fonts

* **Purpose:**
    * I chose specific monospace fonts for improved code readability.
* **Recommended Fonts:**
    * 22) Fira Code
    * 20) EnvyCodeR
    * 15) D2Coding
    * 21) FantasqueSansMono
    * 40) Lekton
    * 36) Iosevka
    * 9) Cascadia Code
    * 39) JetBrains Mono
    * 27) Hack
* **Installation:**
    * Installed the fonts via: `getnf`
    * **Usage:**
    * Set the fonts in my terminal and code editor settings.

## 3. Dash to Panel (GNOME Extension)

* **Purpose:**
    * I use Dash to Panel to combine the GNOME Dash and top panel.
* **Installation:**
    * Installed the GNOME Extensions
    * Installed Dash to Panel through the Extensions app.
* **Usage:**
    * The Dash and top panel are now combined.
    * Configured settings via the Extensions app.


# Topgrade Custom Commands

This section documents the custom commands I've added to my Topgrade configuration for streamlined system maintenance.

## Ollama Updates

* **"Update Ollama"**:
    * Purpose: Automatically checks for and updates Ollama to the latest version.
    * Command:
        ```bash
        bash -c 'current=$(ollama --version 2>&1 | grep -oP "\\d+\\.\\d+\\.\\d+"); latest=$(curl -s [https://api.github.com/repos/ollama/ollama/releases/latest](https://api.github.com/repos/ollama/ollama/releases/latest) | grep -oP "\\"tag_name\\":\\s*\\"v\\K[0-9.]+"); if [[ "$current" != "$latest" ]]; then echo "Upgrading Ollama from $current to $latest"; curl -fsSL [https://ollama.com/install.sh](https://ollama.com/install.sh) | sh; else echo "Ollama $current is already the latest version"; fi'
        ```
    * Functionality:
        * Retrieves the current and latest Ollama versions.
        * Compares the versions and performs an update if necessary.
        * Provides informative output about the update process.
* **"Update Ollama Models"**:
    * Purpose: Automatically updates all locally installed Ollama models.
    * Command:
        ```bash
        ollama list 2>/dev/null | awk 'NR>1 {print $1}' | xargs -I {} sh -c 'echo "Updating model: {}"; ollama pull {} || echo "Failed to update {}"; echo "--"' && echo "All models updated."
        ```
    * Functionality:
        * Lists all installed Ollama models.
        * Iterates through the models and pulls the latest updates.
        * Provides output for each model update attempt, with error reporting.
        * Confirms when all models have been processed.


## Ghostty Configuration Update

This section details an updated Ghostty configuration, focusing on font settings, appearance, and behavior.

**Configuration:**

```toml
# Font Settings
font-family = "FantasqueSansM Nerd Font"
font-size = 11
font-feature = "-calt, -liga, -dlig"
font-synthetic-style = true

# Appearance
theme = "light:rose-pine-dawn,dark:Tomorrow Night Blue"
background-opacity = 0.95
background-blur = false
cursor-style = "block"
cursor-style-blink = false
cursor-opacity = 1.0

# Behavior
command = "zsh"
scrollback-limit = 4194304
mouse-hide-while-typing = true
mouse-scroll-multiplier = 1.0

# Adjustments (Optional, adjust as needed)
adjust-cell-width = 0
adjust-cell-height = 0
adjust-font-baseline = 0
adjust-underline-position = 0
adjust-underline-thickness = 1
adjust-strikethrough-position = 0
adjust-strikethrough-thickness = 1
adjust-overline-position = 0
adjust-overline-thickness = 1
adjust-cursor-thickness = 1
adjust-cursor-height = 1
adjust-box-thickness = 1