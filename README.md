# Debian Sid Post install
```sh
```

# Configure apt
```sh
sudo nano /etc/apt/sources.list
```

# Enabling non-free packages and multimedia repo

```sh
deb http://deb.debian.org/debian/ sid main contrib non-free non-free-firmware
deb https://www.deb-multimedia.org/ sid main non-free
```

# Updating packages
```sh
sudo apt update  &&  sudo apt upgrade
sudo apt-get update -oAcquire::AllowInsecureRepositories=true
sudo apt-get install deb-multimedia-keyring -y --allow-unauthenticated
sudo apt dist-upgrade -y --allow-unauthenticated
```

# Installing multimedia codecs
```sh
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install libglx-mesa0:i386 mesa-vulkan-drivers:i386 libgl1-mesa-dri:i386 mesa-vdpau-drivers mesa-va-drivers mesa-opencl-icd mesa-utils x264 x265 vainfo mpv vlc ffmpeg ffmpegthumbs firmware-amd-graphics libgl1-mesa-dri libglx-mesa0 mesa-vulkan-drivers xserver-xorg-video-all
```

# Installing Gstreamer
```sh
sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
```

# Check if everything installed properly
```sh
vainfo
gst-inspect-1.0 va
```

# Installing pipewire
```sh
sudo apt install pipewire libspa-0.2-bluetooth wireplumber pipewire-pulse pipewire-jack pipewire-alsa pipewire-media-session-
systemctl --user --now enable wireplumber.service
systemctl --user --now enable pipewire
```

# Installing some programs
```sh
sudo apt install wget curl ttf-mscorefonts-installer bleachbit chromium discord mpv vlc qbittorrent thunderbird telegram-desktop keepassxc obs-studio firmware-misc-nonfree firmware-amd-graphics desktop-file-utils firmware-realtek
```

# Fix for grub some bios doesnt detect
```sh
sudo grub-install /dev/nvme0n1p1 --removable
```

# Disabling mitigations enable modern standby in grub
```sh
sudo nano /etc/default/grub
```
```sh
GRUB_CMDLINE_LINUX_DEFAULT="rhgb quiet mitigations=off loglevel=3 mem_sleep_default=s2idle"
```
```sh
sudo update-grub
```

# Some sysctl modifications
```sh
sudo nano /etc/sysctl.conf
```

```sh
kernel.printk = 0 0 0 0
vm.dirty_bytes=50331648
vm.dirty_background_bytes=16777216
vm.dirty_background_ratio=0
vm.dirty_ratio=0
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

# Reduce boot time
```sh
sudo systemctl disable NetworkManager-wait-online.service
```
```sh
sudo nano /etc/systemd/system.conf
```
#Uncomment the DefaultTimeoutStopSec line and change from 90s to 15s
```sh
DefaultTimeoutStopSec=15s
```

# Make Bash look colourful
```sh
nano .bashrc
```

```sh
PS1='\[\033[1;36m\]\u\[\033[1;31m\]@\[\033[1;32m\]\h:\[\033[1;35m\]\w\[\033[1;31m\]\$\[\033[0m\] '
```

```sh
sudo apt install --reinstall bash-completion
```

# Bash ignore case and auto-complete
```sh
echo 'set completion-ignore-case On' >> /etc/inputrc
echo 'set completion-ignore-case On' | sudo tee -a /etc/inputrc
echo 'set show-all-if-ambiguous on' >> /etc/inputrc
echo 'set show-all-if-ambiguous on' | sudo tee -a /etc/inputrc
echo 'TAB:menu-complete' >> /etc/inputrc
echo 'TAB:menu-complete' | sudo tee -a /etc/inputrc
```

# Disabling Out-Of-Memory (optional -not recommended)
```sh
sudo systemctl stop systemd-oomd.service
sudo systemctl disable systemd-oomd.service
sudo systemctl mask systemd-oomd.service
```

# Fix locale for some apps
```sh
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

# Some KDE tweaks

# For Debian sid minimal kde install if networks is not visible

```sh
nano /etc/network/interfaces
```
remove all lines after # The primary network interface
```sh
nano  /etc/NetworkManager/NetworkManager.conf
```
```sh
[ifupdown]
managed=true
```

# Install some kde useful programs
```sh
sudo apt install ark kcalc gwenview kde-spectacle bzip2 p7zip-full unar unzip zip --no-install-recommends --no-install-suggests
```

# Disable kaccess
```sh
systemctl --user disable at-spi-dbus-bus.service
systemctl --user mask at-spi-dbus-bus.service
sudo chmod -x /usr/bin/kaccess
```

# Disable Baloo

```sh
balooctl disable
balooctl purge
sudo rm -rf .local/share/baloo/
```

# Disable Krunner

```sh
sudo chmod -x /usr/bin/krunner
```

# Disable hibernate

```sh
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

