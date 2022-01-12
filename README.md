# chromebook-neovim-console
Transform a Chromebook into a Neovim development platform running in a text TTY console. Minimal Linux installation using Debian 11 netinst non-free firmware version on an Acer Chromebook 14 (CB3-431).

Full stereo sound support
8x12 Terminus font with Powerline symbols
TUI network-manager for wireless connections
Search key working as CAPSLOCK key

###Install Debian from USB:###

On another computer, download 64-bit Debian 11 "netinst" ISO with non-free firmware
Create a USB installer with Etcher
Plug the USB Drive into the Chromebook
Turn on the Chromebook, and boot from the USB drive
During the installation package selection, de-select everything and just keep the "Standard System Utilities" and/or "SSH Server" if you want to remotely log in to this machine. This will keep things nice and light. After installation, your disk usage will be a lean 1.1GB used and 25GB available.

###Setup Wireless Connection:###

As root user, use the commands below to find the name of your wireless device and ethernet device, in my case it was wlp2s0 (normally wlan0):

ip addr show

Bring your wireless device up with:

ip link set wlp2s0 up

Find your wireless router SSID with:

iwlist wlp2s0 scan | grep ESSID

Edit the /etc/network/interfaces file with nano and make sure the section below is present:

allow-hotplug wlp2s0
iface wlp2s0 inet dhcp
     wpa-ssid "your_ssid"
     wpa-psk "your_password"
Now you can activate your wireless device with ifup and ifdown.

ifup wlp2s0

Now test your setup with:

ping 8.8.8.8

Once Internet connection is working, now is a good time to make sure the system is updated to the latest packages:

apt-get update
apt-get upgrade

###Running the Configuration Script###
