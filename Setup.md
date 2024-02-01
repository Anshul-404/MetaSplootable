# Setup Process
------------------------------

## Setting up docker and Metsploitable2
---------------------------------------
- Install docker on pi
- Install metasploitable on docker
	- `docker search metasploitable`
	- `docker pull <first_result>`

## Setting Up interface on Kali and docker
------------------------------------------
- Enable 'dummy' kernel module:
	- `sudo modprobe dummy`
- Creating a new virtual interface (You can use this interface name or any other you want):
	- `sudo ip link add metspi0 type dummy`
- Verify if it is created:
	- `ip link show metspi0`
- Give interface MAC address (You can use this address or any other you want):
	- `sudo ifconfig metspi0 hw ether C8:D7:4A:4E:47:69`
- Give IP address to interface:
	- `sudo ipconfig metspi0 192.168.1.69` 

- Making it persistent across reboots (Above configuration doesn't retain on reboots):
	- `sudo nano /etc/modules`
		- Add `dummy` at bottom for auto load on boot
	- `sudo nano /etc/network/if-up.d/dummy-interface`
	- Add the following lines to the script (replace metspi0 and the IP address with your preferred interface name and IP configuration):
		- `#!/bin/sh`
		- `sudo ip link add metspi0 type dummy`
		- `sudo ifconfig metspi0 hw ether C8:D7:4A:4E:47:69`
		- `sudo ipconfig metspi0 192.168.1.69`
		- `ip link set metspi0 up`
	- `sudo chmod +x /etc/network/if-up.d/dummy-interface`
	- `sudo reboot`

- Use docker-compose.yml to start docker and configure ports  

### Note: 
 - You must do this on both your Kali Machine (attacking system) and PI (metasploitable2 machine).
 - You have to use different IPs for both Kali and metasploitable2 machine.


### OR

- Download and install VMware and boot the metasploitable image from https://sourceforge.net/projects/metasploitable/files/latest/download -> easier option fr