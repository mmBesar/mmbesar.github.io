---
template: blog_post.html
title: NixOS - Minimal Install - GNOME
description: NixOS | تخصيص تنصيب توزيعة Nix بواجهة GNOME
date: 2023-08-31
---

# <div dir="rtl">NixOS | توزيعة منيعة من المستقبل - تنصيب مُتقدم بواجهة جنوم</div>

![type:video](https://www.youtube.com/embed/vQLssnUXKj0)

<div dir="rtl">
تقدم توزيعة NixOS حلولًا متطورة للأجهزة المكتبية والخوادم على حد سواء، التوزيعة منيعة، حيث تحفظ قلب النظام معزولًا لتجنب الأخطاء، وتحتفظ بنقاط عودة عند أي تعديل، لضمان امكانية استعادة النظام حال حدوث أي مشكلة. 
</div>
<div dir="rtl">
يمكن إعادة إنتاج النظام بنفس التطبيقات والإعدادات على أي جهاز، باستخدام ملف اعدادات واحد. 
</div>
<div dir="rtl">
شرح خطوات التنصيب المُتقدم لتوزيعة NixOS بواجهة GNOME.  
</div>

<p hidden>#more</p>

## Install NixOS

### Partitioning and Formatting on UEFI without SWAP

1. List all Disks

```sh
lsblk
```

2. Create a GPT partition table.
```sh
parted /dev/vda -- mklabel gpt
```
        
3. Add the root partition. This will fill the disk and the space left in front (1GiB) which will be used by the boot partition.

``` sh
parted /dev/vda -- mkpart primary 1GiB 100%
```

4. Finally, the boot partition. NixOS by default uses the ESP (EFI system partition) as its /boot partition. It uses the initially reserved 1GiB at the start of the disk.

```sh
parted /dev/vda -- mkpart ESP fat32 1MiB 1GiB
parted /dev/vda -- set 2 esp on
```
       
5. Format `vda1` as root
```sh
mkfs.ext4 -L nixos /dev/vda1
```
            
6. Format `vda1` as boot efi
```sh
mkfs.fat -F 32 -n boot /dev/vda2
```

## Mount Partitions

1. Mount the target file system on which NixOS should be installed on /mnt, e.g.

```sh
mount /dev/disk/by-label/nixos /mnt
```

2. Mount the boot file system on /mnt/boot, e.g. 

```sh
mkdir -p /mnt/boot
mount /dev/disk/by-label/boot /mnt/boot
```

## Generate the Configuration file

```sh
nixos-generate-config --root /mnt
```

## Customize the Configuration file

```sh
nano /mnt/etc/nixos/configuration.nix
```

``` nix title="configuration.nix"
# Edit this configuration file to define what should be installed on
# your system.  Help is available in the configuration.nix(5) man page
# and in the NixOS manual (accessible by running `nixos-help`).

{ config, pkgs, ... }:

{
  imports =
    [ # Include the results of the hardware scan.
      ./hardware-configuration.nix
    ];

  # Use the systemd-boot EFI boot loader.
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  networking.hostName = "nixos-vm"; # Define your hostname.
  # Pick only one of the below networking options.
  # networking.wireless.enable = true;  # Enables wireless support via wpa_supplicant.
  networking.networkmanager.enable = true;  # Easiest to use and most distros use this by default.

  # Set your time zone.
  time.timeZone = "Africa/Cairo";

  # Configure network proxy if necessary
  # networking.proxy.default = "http://user:password@proxy:port/";
  # networking.proxy.noProxy = "127.0.0.1,localhost,internal.domain";

  # Select internationalisation properties.
  i18n.defaultLocale = "en_US.UTF-8";

  i18n.extraLocaleSettings = {
    LC_ADDRESS = "ar_EG.utf8";
    LC_IDENTIFICATION = "ar_EG.utf8";
    LC_MEASUREMENT = "ar_EG.utf8";
    LC_MONETARY = "ar_EG.utf8";
    LC_NAME = "ar_EG.utf8";
    LC_NUMERIC = "ar_EG.utf8";
    LC_PAPER = "ar_EG.utf8";
    LC_TELEPHONE = "ar_EG.utf8";
    LC_TIME = "ar_EG.utf8";
  };

  # console = {
  #   font = "Lat2-Terminus16";
  #   keyMap = "us";
  #   useXkbConfig = true; # use xkbOptions in tty.
  # };

  # Enable the X11 windowing system.
  services.xserver.enable = true;

  # Enable the GNOME Desktop Environment.
  services.xserver.displayManager.gdm.enable = true;
  services.xserver.desktopManager.gnome.enable = true;
  

  # Configure keymap in X11
  services.xserver.layout = "us,ara";
  services.xserver.xkbVariant = "";
  services.xserver.xkbOptions = "grp:win_space_toggle";
  # services.xserver.xkbOptions = "eurosign:e,caps:escape";

  # For GNOME, If layout updated after installation, open a terminal and run:
  # gsettings reset org.gnome.desktop.input-sources xkb-options
  # gsettings reset org.gnome.desktop.input-sources sources

  # Enable CUPS to print documents.
  services.printing.enable = true;

  # Enable sound.
  sound.enable = true;
  hardware.pulseaudio.enable = false;
  security.rtkit.enable = true;
  services.pipewire = {
    enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    pulse.enable = true;
    # If you want to use JACK applications, uncomment this
    #jack.enable = true;

    # use the example session manager (no others are packaged yet so this is enabled by default,
    # no need to redefine it in your config for now)
    #media-session.enable = true;
  };

  # Enable touchpad support (enabled default in most desktopManager).
  # services.xserver.libinput.enable = true;

  # Define a user account. Don't forget to set a password with ‘passwd’.
  users.users.mbesar = {
    isNormalUser = true;
    initialPassword = "123";
    extraGroups = [ "networkmanager" "wheel" ]; # Enable ‘sudo’ for the user.
  #   packages = with pkgs; [
  #     firefox
  #     tree
  #   ];
  };

  # List packages installed in system profile. To search, run:
  # $ nix search wget
  environment.systemPackages = with pkgs; [
    vim # Do not forget to add an editor to edit configuration.nix! The Nano editor is also installed by default.
    wget
    firefox
    htop
    git
  ];

  # Some programs need SUID wrappers, can be configured further or are
  # started in user sessions.
  # programs.mtr.enable = true;
  # programs.gnupg.agent = {
  #   enable = true;
  #   enableSSHSupport = true;
  # };

  # List services that you want to enable:

  # Enable the OpenSSH daemon.
  services.openssh.enable = true;

  # Enable the qemuGuest daemon.
  services.qemuGuest.enable = true;

  # Enable the spice-vdagentd daemon.
  services.spice-vdagentd.enable = true;

  # ZRAM SWAP
  zramSwap = {
    enable = true;
    algorithm = "zstd";
    memoryPercent = 80;
  };

  # Open ports in the firewall.
  # networking.firewall.allowedTCPPorts = [ ... ];
  # networking.firewall.allowedUDPPorts = [ ... ];
  # Or disable the firewall altogether.
  # networking.firewall.enable = false;

  # Copy the NixOS configuration file and link it from the resulting system
  # (/run/current-system/configuration.nix). This is useful in case you
  # accidentally delete configuration.nix.
  # system.copySystemConfiguration = true;

  # This value determines the NixOS release from which the default
  # settings for stateful data, like file locations and database versions
  # on your system were taken. It's perfectly fine and recommended to leave
  # this value at the release version of the first install of this system.
  # Before changing this value read the documentation for this option
  # (e.g. man configuration.nix or on https://nixos.org/nixos/options.html).
  system.stateVersion = "23.05"; # Did you read the comment?

}
```

## Run the Installer

```sh
nixos-install
```