+++
title = "Fixing Wi-Fi on Alpine"
date = 2021-08-23
description = "Getting Wi-Fi working again on Alpine Linux after I broke it somehow"
[taxonomies]
tags = ["linux","alpine","networking"]
+++

Note that this was definitely broken due to something I did. During the initial setup of Alpine, Wi-Fi worked fine, and everytime I use the boot USB for recovery etc, it works fine there as well. The fault definitely lies with me here.

I don't know since when this was a problem as I've just been using an Ethernet cable, but today I decided to fix `wlan0` not working in my Alpine. Here is what I did:

1. Try running `setup-interfaces`, notice that it's not even offered as an option
2. Look into `dmesg`, find two problems
    1. Firstly, it was complaining about `regulatory.db`. Fixed with `apk add wireless-regdb`
    2. Secondly, it was complaining about missing `iwlwifi` firmware. Fixed as follows:
        1. Clone [Linux's firmware](https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/linux-firmware.git/)
        2. `cp linux-firmware/iwlwifi-9000-pu-b0-jf-b0-46.ucode /lib/firmware/`
3. Reboot. Check `dmesg`, no issues. Can now run `setup-interfaces`
4. Try running `wpa_cli status`, fails.
5. After some googling, find I need to modify two files:
    /etc/network/interfaces

		```
		...
		iface wlan0 inet dhcp
		# added below line
		wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
		```
		
		/etc/wpa_supplicant/wpa_supplicant.conf
		
		```
		# added these 3 lines
		country=SK
		ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
		update_config=1
		p2p_disabled=1
		
		network={
		....
		```
6. Add your user to the `netdev` group with `adduser <username> netdev>`
7. Reboot, then run `wpa_cli status`, should be all good now
8. Install `wifish` for a nice frontend

