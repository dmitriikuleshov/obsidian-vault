
> A clean Arch Linux installation checklist based on the commands used during setup.  
> Target setup: **KDE Plasma**, **Btrfs**, **GRUB**, **NetworkManager**, **PipeWire**, and common development tools.

---

## 1. Connect to Wi-Fi in the Live ISO

Start the interactive Wi-Fi utility:

```bash
iwctl
```

`iwctl` is used to connect to Wi-Fi from the Arch Linux live environment.

### Check available Wi-Fi adapters

```bash
adapter list
```

Shows available wireless adapters.

### Power on the Wi-Fi adapter

```bash
adapter phy0 set-property Powered on
```

Enables the physical Wi-Fi adapter.

### Check available wireless devices

```bash
device list
```

Shows available wireless devices, usually something like `wlan0`.

### Power on the wireless device

```bash
device wlan0 set-property Powered on
```

Enables the Wi-Fi device itself.

### Show available Wi-Fi networks

```bash
station wlan0 get-networks
```

Scans and lists nearby Wi-Fi networks.

### Connect to your Wi-Fi network

```bash
station wlan0 connect <network>
```

Replace `<network>` with the name of your Wi-Fi network.

Example:

```bash
station wlan0 connect MyWiFiName
```

You may be asked to enter the Wi-Fi password.

### Exit `iwctl`

```bash
exit
```

Returns you to the normal shell.

---

## 2. Check Internet Connection

```bash
ping -c 5 google.com
```

Sends 5 packets to `google.com` to confirm that the internet connection works.

If you see replies, the connection is working.

---

## 3. Update Package Database

```bash
pacman -Sy
```

Synchronizes the package database with the Arch Linux repositories.

---

## 4. Install Arch Installer Tools

```bash
pacman -S archlinux-keyring archinstall
```

Installs the required packages for installation:

- `archlinux-keyring` — updates Arch Linux package signing keys.
- `archinstall` — the official guided Arch Linux installer.

---

## 5. Check Available Disks

```bash
lsblk
```

Shows all disks and partitions.

Use this command to identify the disk where Arch Linux will be installed.

Examples of disk names:

```text
/dev/nvme0n1
/dev/sda
```

> [!WARNING]
> Be very careful when choosing the target disk. Selecting the wrong disk can erase important data.

---

## 6. Erase the Target Disk with `gdisk`

Open the target disk in `gdisk`:

```bash
gdisk /dev/<disk>
```

Replace `<disk>` with your real disk name.

Example:

```bash
gdisk /dev/nvme0n1
```

### Enter expert mode

Inside `gdisk`, type:

```text
x
```

This enters expert mode.

### Erase the disk

Inside expert mode, type:

```text
z
```

This erases the partition table from the selected disk.

> [!DANGER]
> This is destructive. Make sure the selected disk is correct before confirming.

---

## 7. Start the Arch Linux Installer

```bash
archinstall
```

Starts the official guided Arch Linux installer.

---

## 8. Configure `archinstall`

Use the following options inside the installer.

### Disk Configuration

```text
Disk configuration
└─ Partitioning
   └─ Use a best-effort default partition layout
      └─ nvme0n1
         └─ btrfs
            ├─ subvolumes: yes
            └─ use compression
```

This creates a default partition layout using **Btrfs**.

Btrfs subvolumes are useful for snapshots, backups, and rollback functionality. Compression can save disk space and may improve performance on SSDs.

---

### Swap

```text
Swap -> Enable swap
```

Enables swap space.

Swap is useful when RAM is full and can help improve system stability.

---

### Bootloader

```text
Bootloader -> GRUB
```

Installs **GRUB** as the bootloader.

The bootloader is responsible for starting Arch Linux when the computer turns on.

---

### Root Password

```text
Authentication -> Root password
```

Sets the password for the `root` account.

The root account has full administrative access to the system.

---

### Create User

```text
Authentication -> Add user -> Add to sudo
```

Creates a normal user and gives it administrator permissions through `sudo`.

This allows you to run administrative commands without logging in as `root`.

---

### Desktop Environment

```text
Profile -> Desktop -> KDE Plasma
```

Installs **KDE Plasma** as the graphical desktop environment.

KDE Plasma is a modern, customizable Linux desktop.

---

### Graphics Driver

```text
Profile -> Graphics driver -> AMD
```

Selects AMD graphics drivers.

Use this option if your system has an AMD GPU.

---

### Applications

```text
Applications -> Bluetooth -> Enable
Applications -> Audio -> PipeWire
```

Enables Bluetooth support and installs **PipeWire** for audio.

PipeWire is the modern audio and multimedia server commonly used on Linux desktops.

---

### Network Configuration

```text
Network configuration -> Use NetworkManager
```

Uses **NetworkManager** as the default network backend.

NetworkManager makes it easier to manage Wi-Fi and wired connections after installation.

---

### Timezone

```text
Timezone -> Moscow
```

Sets the system timezone to Moscow.

---

### Install the System

```text
Install
```

Starts the installation using the selected configuration.

---

## 9. Enter the Installed System

After installation finishes, select:

```text
chroot into installation for post-installation configuration
```

This enters the newly installed Arch Linux system before rebooting.

It allows you to install extra packages and apply additional configuration.

---

## 10. Install Common Packages

```bash
sudo pacman -S gcc make cmake git nano htop flatpak wget curl power-profiles-daemon chromium libreoffice ghostty fish keepassxc telegram-desktop uv docker code base-devel obsidian timeshift dbeaver
```

This installs useful development, desktop, and productivity packages.

### Package overview

| Package | Purpose |
|---|---|
| `gcc` | C/C++ compiler |
| `make` | Build automation tool |
| `cmake` | Cross-platform build system |
| `git` | Version control system |
| `nano` | Simple terminal text editor |
| `htop` | Interactive system monitor |
| `flatpak` | Universal Linux application format |
| `wget` | Command-line file downloader |
| `curl` | Command-line tool for transferring data |
| `power-profiles-daemon` | Power profile management |
| `chromium` | Web browser |
| `libreoffice` | Office suite |
| `ghostty` | Terminal emulator |
| `fish` | Friendly interactive shell |
| `keepassxc` | Password manager |
| `telegram-desktop` | Telegram desktop client |
| `uv` | Python package and project manager |
| `docker` | Container platform |
| `code` | Visual Studio Code |
| `base-devel` | Required tools for building AUR packages |
| `obsidian` | Markdown note-taking app |
| `timeshift` | System snapshot and restore tool |
| `dbeaver` | Database management tool |

> [!NOTE]
> In the original command, `pacan` looked like a typo. It was removed because the correct package manager is `pacman`, not a package named `pacan`.

---

## 11. Install `yay` AUR Helper

Clone the `yay` package from the AUR:

```bash
git clone https://aur.archlinux.org/yay.git
```

Enter the directory:

```bash
cd yay
```

Build and install `yay`:

```bash
makepkg -si
```

`yay` is an AUR helper. It allows you to install packages from the Arch User Repository more easily.

> [!NOTE]
> The correct URL for `yay` is `https://aur.archlinux.org/yay.git`.

---

## 12. Install AUR Packages

Install AmneziaVPN:

```bash
yay -S amneziavpn-bin
```

Installs the binary version of AmneziaVPN from the AUR.

Install Postman:

```bash
yay -S postman-bin
```

Installs the binary version of Postman from the AUR.

---

## 13. Exit Chroot

```bash
exit
```

Leaves the installed system environment and returns to the live ISO shell.

---

## 14. Shut Down

```bash
shutdown now
```

Powers off the machine.

After shutdown, remove the installation USB drive and boot into the installed Arch Linux system.

---

# Post-Installation Configuration

After booting into the installed system, log in with your user account.

---

## 15. Change Default Shell to Fish

```bash
sudo chsh -s $(which fish) <user>
```

Changes the default shell for `<user>` to Fish.

Replace `<user>` with your actual username.

Example:

```bash
sudo chsh -s $(which fish) alex
```

Log out and log back in for the change to take effect.

---

## 16. Configure Login Failure Lockout

Open the `faillock` configuration file:

```bash
sudo nano /etc/security/faillock.conf
```

Find or add this line:

```text
deny = 100000
```

This changes how many failed login attempts are allowed before the account gets locked.

> [!WARNING]
> A very high value makes accidental lockouts less likely, but it also weakens brute-force protection.

Save and exit Nano:

```text
Ctrl + O
Enter
Ctrl + X
```

---

## 17. Enable `systemd-resolved`

```bash
sudo systemctl enable --now systemd-resolved
```

Enables and starts `systemd-resolved` immediately.

`systemd-resolved` provides DNS name resolution for the system.

---

# Final Checklist

- [x] Wi-Fi connected in the live ISO
- [x] Internet connection checked
- [x] Arch installer installed
- [x] Disk selected and wiped
- [x] Arch Linux installed with Btrfs
- [x] Swap enabled
- [x] GRUB installed
- [x] KDE Plasma installed
- [x] AMD graphics driver selected
- [x] Bluetooth enabled
- [x] PipeWire installed
- [x] NetworkManager enabled
- [x] User created with sudo access
- [x] Common packages installed
- [x] `yay` installed
- [x] AUR packages installed
- [x] Fish shell configured
- [x] `systemd-resolved` enabled

---

# Notes

Use this note as a personal Arch Linux installation reference.

Before running destructive disk commands, always check the target disk with:

```bash
lsblk
```
