# chromebook-neovim-console
Transform a Chromebook into a Neovim development platform running in a text TTY console. Minimal Linux installation using Debian 11 netinst non-free firmware version on an Acer Chromebook 14 (Model CB3-431-12K1, Hardware ID: EDGAR) based on the Intel Braswell architecture. It has a full aluminum alloy housing, 14-inch HD screen (1366x768), an Intel Atom x5-E8000 (Quad-Core 1.04GHz, Turbo 2.0GHz, 2MB Cache), 4GB DDR3 RAM, and 32GB eMMC storage.

- Full stereo sound support
- 8x12 Terminus font with Powerline symbols
- TUI network-manager for wireless connections
- Search key working as CAPSLOCK key

### Flash a Full UEFI Firmware ROM

Open the backside of the Chromebook (10 screws) and remove the WP screw as shown below:

![alt text](https://blogger.googleusercontent.com/img/a/AVvXsEgGiekfZdsTWjjoi6dxTJ76_bxxlMBPdMQxtWiaQJHTLoYWmyxHZ6wGyYPex2wC3XrSjFCinZ5WlXx9PBQtmzVNAkr6YPA3NPVoX_rB5mwrEWe9GWzGTGHyT-bfodF_O0Tj6ui8BY9H-Uw-4PTzwT8xcX36uAPBWJLGWsnfYhudkUZ3BDiHka77wsnv=s1630 "WP Screw Location")

Now we enable Developer Mode in order to flash a new firmware:

- Turn on your device with `Esc + Refresh + Power`
- Press `Ctrl + D` and press Enter to enable developer mode
- Press `Ctrl + D` to boot Chrome OS
- Open a browser tab and press `Ctrl + Alt + T`
- Type in "shell" and press `Enter`

Paste the following into the shell and press enter:

```
cd; curl -LO https://mrchromebox.tech/firmware-util.sh && sudo bash firmware-util.sh
```

- Select "Install/Update Full ROM Firmware" from the options.

Go grab a cup of coffee and wait for your Chromebook's emancipation from Chrome OS.

### Install Debian from USB

On another computer, download 64-bit Debian 11 "netinst" ISO with non-free firmware

- Create a USB installer with Etcher
- Plug the USB Drive into the Chromebook
- Turn on the Chromebook, and boot from the USB drive

During the installation package selection, de-select everything and just keep the "Standard System Utilities" and/or "SSH Server" if you want to remotely log in to this machine. This will keep things nice and light. After installation, your disk usage will be a lean 1.1GB used and 25GB available.

### Setup Wireless Connection

As root user, use the commands below to find the name of your wireless device and ethernet device, in my case it was *wlp2s0* (normally *wlan0*):

`ip addr show`

Bring your wireless device up with:

`ip link set wlp2s0 up`

Find your wireless router SSID with:

`iwlist wlp2s0 scan | grep ESSID`

Edit the */etc/network/interfaces* file with `nano` and add the section below:
```
allow-hotplug wlp2s0
iface wlp2s0 inet dhcp
     wpa-ssid "your_ssid"
     wpa-psk "your_password"
```
Now you can activate your wireless device with `ifup` and `ifdown`.

`ifup wlp2s0`

Now test your setup with:

`ping 8.8.8.8`

Once Internet connection is working, now is a good time to make sure the system is updated to the latest packages:
```
apt-get update
apt-get upgrade
```
### Running the Configuration Script
