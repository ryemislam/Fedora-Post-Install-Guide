# Fedora 42 Post Install Guide
Things to do after installing Fedora 42

## DNF default Yes

```
sudo nano /etc/dnf/dnf.conf
```
Add this line:
```
defaultyes=True
```

## RPM Fusion

* Fedora has disabled the repositories for a lot of free and non-free .rpm packages by default. Follow this if you want to use non-free software like Steam, Discord and some multimedia codecs etc. As a general rule of thumb it is advised to do this to get access to many mainstream useful programs.
* Enable third party repositories by pasting the following into the terminal: 
```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
- Update core:
```
sudo dnf group upgrade core
```
- Update: 
```
sudo dnf -y update
```
```
sudo dnf -y upgrade --refresh
```
```
sudo reboot
```

## Flatpak
* Fedora doesn't include all non-free flatpaks by default. The command below enables access to all the flathub flatpaks. Particularly useful for users of Fedora KDE and other spins since they do not get the "Enable Third Party Repositories" option on initial boot.
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
## NVIDIA Drivers

* Update system to make sure you're on the latest kernel and then reboot:
```
sudo dnf update
```

```
sudo dnf install akmod-nvidia
```
- If you use CUDA, install this:
```
sudo dnf install xorg-x11-drv-nvidia-cuda
```
* Wait for 2 mins before rebooting, to let the kernel module get built.
* Check if the kernel module is built.
```
modinfo -F version nvidia
```
* Reboot
## Media Codecs
* Install these to get proper multimedia playback.
````
sudo dnf group install multimedia
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing # Switch to full FFMPEG.
sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin # Installs gstreamer components. Required if you use Gnome Videos and other dependent applications.
sudo dnf group install -y sound-and-video # Installs useful Sound and Video complementary packages.
````

## H/W Video Acceleration

* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Quite helpful in increasing battery backup on laptops.

### H/W Video Decoding with VA-API 

```
sudo dnf install ffmpeg-libs libva libva-utils
```

## Intel

* If you have a recent Intel chipset (5th Gen and above) after installing the packages above., Do:
* `sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing`
* `sudo dnf install libva-intel-driver`

## AMD

No need to do this for intel integrated graphics. Mesa drivers are for AMD graphics, who lost support for h264/h265 in the fedora repositories in f38 due to legal concerns.
 
* If you have an AMD chipset, after installing the packages above do:
```
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
sudo dnf swap mesa-va-drivers.i686 mesa-va-drivers-freeworld.i686
sudo dnf swap mesa-vdpau-drivers.i686 mesa-vdpau-drivers-freeworld.i686
```
## Set Hostname

```
hostnamectl set-hostname YOUR_HOSTNAME
```
## Optimizations
##### The tips below can allow you to squeeze out a little bit more performance from your system. 

### Default Firefox start page 
* The tweak below will make the start page the default firefox start page instead of [this](https://fedoraproject.org/start)
```
sudo rm -f /usr/lib64/firefox/browser/defaults/preferences/firefox-redhat-default-prefs.js
```

### Disable `NetworkManager-wait-online.service`
* Disabling it can decrease the boot time by at least ~15s-20s:
```
sudo systemctl disable NetworkManager-wait-online.service
```

# [Optional]
## Enable nvidia-modeset  [Optional]
* Useful if you have a laptop with an Nvidia GPU. Necessary for some PRIME-related interoperability features.
* `sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"`
## Apps

#### Essentials
```
sudo dnf install btop deskflow goverlay iperf3 kitty kate steam syncthing micro mpv
```

#### Fish Shell
```
sudo dnf install fish
```
```
chsh -s /usr/bin/fish
```

#### LACT:
Lact allows you to control your AMD, Nvidia or Intel GPU on a Linux system.

```
sudo dnf copr enable ilyaz/LACT
```
```
sudo dnf install lact
```
```
sudo systemctl enable --now lactd
```

#### Sunshine:
Low Latency Remote Gaming

```
sudo dnf copr enable lizardbyte/beta
```
```
sudo dnf install sunshine
```
```
systemctl --user enable sunshine.service
systemctl --user start sunshine.service
```

#### AB-Download-Manager

```
bash <(curl -fsSL https://raw.githubusercontent.com/amir1376/ab-download-manager/master/scripts/install.sh)
```

### Flatpaks


`Bitwarden, Bottles, Czkawka, Fan Control, Fooyin, FreeFileSync, Github Desktop, Heroic Games Launcher, Mission Center, Moonlight, Obsidian, OBS Studio, ONLYOFFICE Desktop Editors, qBittorrent, Remote Desktop Manager, Resources, Spotify, Stream Controller, Vesktop, ZapZap`

```
flatpak install flathub com.bitwarden.desktop com.usebottles.bottles com.github.qarmin.czkawka io.github.wiiznokes.fan-control org.fooyin.fooyin org.freefilesync.FreeFileSync com.heroicgameslauncher.hgl io.missioncenter.MissionCenter com.moonlight_stream.Moonlight md.obsidian.Obsidian com.obsproject.Studio org.onlyoffice.desktopeditors org.qbittorrent.qBittorrent com.devolutions.remotedesktopmanager net.nokyan.Resources io.github.shiftey.Desktop com.spotify.Client com.core447.StreamController dev.vencord.Vesktop com.rtosta.zapzap
```

### My Favourite Apps

| Category             | App Name                                                 |
| -------------------- | -------------------------------------------------------- |
| Launcher             | albert                                                   |
| Download Manager     | brisk, ab-download-manager                               |
| Password Manager     | bitwarden                                                |
| FAN                  | coolercontrol, fancontrol                                |
| Duplicate Finder     | czkawka                                                  |
| Photo Editor         | darktable                                                |
| Video Editor         | davinci-resolve, kdenlive                                |
| KVM                  | deskflow                                                 |
| Music Player         | fooyin, elisa, spotify                                   |
| Terminal             | kitty                                                    |
| Git                  | github-desktop                                           |
| Game Stats           | goverlay                                                 |
| Games                | heroic-games-launcher, steam                             |
| Bootable USB         | isoimagewriter, balena etcher                            |
| System Monitor Tools | btop, mission-center, resources                          |
| GPU Monitor          | lact                                                     |
| File Manager         | krusader, dolphin                                        |
| Phone                | kdeconnect                                               |
| Text Editor          | kate                                                     |
| Disk Benchmark       | kdiskmark                                                |
| Desktop Environment  | KDE                                                      |
| Notes                | obsidian                                                 |
| Store                | octopi, paru (Arch only)                                 |
| Office               | onlyoffice                                               |
| RGB                  | openrgb                                                  |
| Compatibility Tool   | ProtonPlus                                               |
| Mirror Android       | Scrcpy                                                   |
| Remote Desktop       | moonlight-qt, sunshine, rustdesk, remote-desktop-manager |
| Streaming            | obs-studio                                               |
| Torrent              | qbittorrent                                              |
| Stream Deck          | stream-controller                                        |
| Sync Tool            | syncthing, freefilesync                                  |
| Social Media         | vesktop, zapzap                                          |
| Encryption           | veracrypt                                                |
| Browser              | vivaldi, firefox                                         |
| Video Player         | mpv, vlc                                                 |
| Virtual Machine      | virt-manager                                             |

### Gnome Extensions
* Suggestions for good utilities to extend the capabilities of your system
* Don't install these if you are using a different spin of Fedora.
* Pop Shell - run `sudo dnf install -y gnome-shell-extension-pop-shell xprop` to install it.
* [GSconnect](https://extensions.gnome.org/extension/1319/gsconnect/) - run `sudo dnf install nautilus-python` for full support. then `sudo firewall-cmd --permanent --zone=public --add-service=kdeconnect`
* [Gesture Improvements](https://extensions.gnome.org/extension/4245/gesture-improvements/)
* [Quick Settings Tweaker](https://github.com/qwreey75/quick-settings-tweaks)
* [User Themes](https://extensions.gnome.org/extension/19/user-themes/)
* [Compiz Windows Effect](https://extensions.gnome.org/extension/3210/compiz-windows-effect/)
* [Just Perfection](https://extensions.gnome.org/extension/3843/just-perfection/)
* [Rounded Windows Corners](https://extensions.gnome.org/extension/5237/rounded-window-corners/)
* [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
* [Quick Settings Tweaker](https://extensions.gnome.org/extension/5446/quick-settings-tweaker/)
* [Blur My Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/)
* [Bluetooth Quick Connect](https://extensions.gnome.org/extension/1401/bluetooth-quick-connect/)
* [App Indicator Support](https://extensions.gnome.org/extension/615/appindicator-support/)
* [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
* [Legacy (GTK3) Theme Scheme Auto Switcher](https://extensions.gnome.org/extension/4998/legacy-gtk3-theme-scheme-auto-switcher/)
* [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
* [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
* [Wireless HID](https://extensions.gnome.org/extension/4228/wireless-hid/)
* [Logo Menu](https://extensions.gnome.org/extension/4451/logo-menu/)
* [Space Bar](https://github.com/christopher-l/space-bar)
## Theming

### GTK Themes
* Don't install these if you are using a different spin of Fedora.
* https://github.com/lassekongo83/adw-gtk3
* https://github.com/vinceliuice/Colloid-gtk-theme
* https://github.com/EliverLara/Nordic
* https://github.com/vinceliuice/Orchis-theme
* https://github.com/vinceliuice/Graphite-gtk-theme

### KDE Themes
- https://store.kde.org/p/2305670
- https://store.kde.org/p/2053458
- https://store.kde.org/p/1326896
- https://store.kde.org/p/2222239
### Use themes in Flatpaks
* `sudo flatpak override --filesystem=$HOME/.themes`
* `sudo flatpak override --env=GTK_THEME=my-theme` 

### Starship (terminal theme)
* Configure starship to make your terminal look good (refer https://starship.rs)
##### Prerequisites
- A [Nerd Font](https://www.nerdfonts.com/) installed and enabled in your terminal (for example, try the [FiraCode Nerd Font](https://www.nerdfonts.com/font-downloads)).
```
curl -sS https://starship.rs/install.sh | sh
```
- Or you can use COPR:
```
sudo dnf copr enable atim/starship
```
```
sudo dnf install starship
```
```
starship preset catppuccin-powerline -o ~/.config/starship.toml
```

### Grub Theme
* https://github.com/vinceliuice/grub2-themes

# Fedora VM GPU Passthrough

#### Install packages:
```
sudo dnf group install --with-optional virtualization
```
#### Add user to kvm, libvirt groups
```
sudo usermod -a -G kvm,libvirt $(whoami)
```
#### Enable and start Libvirtd
```
sudo systemctl enable --now libvirtd
sudo systemctl start libvirtd
```
#### Start VM Network
```
sudo virsh net-autostart default
sudo virsh net-start default
```
#### Find the ID for your GPU
```
lspci -nnk
```
#### Add the PCI IDS to GRUB
```
sudo micro /etc/sysconfig/grub
```
#### Add the following line at the end of GRUB_CMDLINE_LINUX
```
iommu=pt kvm.ignore_msrs=1 rd.driver.pre=vfio-pci video=efifb:off vfio-pci.ids=1002:73bf,1002:ab28,1002:73a6,1002:73a4
```
#### Regenerate Grub
```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```
#### Add vfio drivers to Dracut
```
sudo micro /etc/dracut.conf.d/local.conf
```
#### Add the following line and save file
```
add_drivers+=" vfio vfio_iommu_type1 vfio_pci "
```
#### Regenerate Dracut
```
sudo dracut -f --kver $(uname -r)
```

```
sudo reboot
```

# Permanently Set SELinux to Permissive

```
sudo micro /etc/selinux/config
```
Find the line:
```
SELINUX=enforcing
```
and change it to:
```
SELINUX=permissive
```
Save the file and exit.
```
sudo reboot
```

```
Disable (SELINUX=disabled) is also possible, but not recommended unless necessary.
```

# Clean Orphaned/Uneeded Packages; Clean downloaded cache files

```
sudo dnf autoremove -y
sudo dnf clean all
```

# Flatpak cleanup commands
Removes unused runtimes:
```
flatpak uninstall --unused
```
Cleans up leftover data, old versions:
```
flatpak cleanup
```
To remove old versions of an app (if you have updated):
```
flatpak remove --unused
```
