
# Raspberry setup

Tested with
* Raspberry Pi 3 Model B
* 2018-11-13-raspbian-stretch-lite

```
$ cat /proc/device-tree/model
Raspberry Pi 3 Model B Rev 1.2%
$ cat /etc/debian_version
9.6
```

## Install Raspbian Stretch Lite

Download the latest image from https://www.raspberrypi.org/downloads/raspbian/  
Flash the image into the micro SD card.

	sudo dd if=2018-11-13-raspbian-stretch-lite.img of=/dev/disk2 bs=2m

Create a file `ssh` in the root of the `boot` partition so SSH service is enabled by default.  
https://www.raspberrypi.org/documentation/remote-access/ssh/README.md#3-enable-ssh-on-a-headless-raspberry-pi-add-file-to-sd-card-on-another-machine

## Internal hostname

The raspberry pi hostname `raspi` is registered in the internal network.  
Changed the hostname in your router configuration.

## Generate SSH keys

This allows to connect via SSH without requiring a password

```
// generate RSA public and private keys
ssh-keygen -t rsa -b 4096

// copy public key in /home/pi/.ssh/authorized_keys
ssh-copy-id pi@raspi

// ssh into the raspi to make sure all is working
ssh pi@raspi
```

## Change default password

Change the default password using `passwd`

# Ansible playbook

The playbook will perform these actions:

* Expand root filesystem
* Overclock raspberry pi 

  **NOTE** voids warranty!!  
  Requires heatsink and case with fan.

* Set the hostname
* Set the timezone 
* Set the keyboard layout and model
* Mounts an external HDD
* Updates packages
* Install and configure Z shell
* Install docker and docker-compose

## Run ansible playbook
 
```
export ANSIBLE_INVENTORY=$(readlink -f hosts)
ansible-playbook playbooks/raspi-bootstrap.yml
```
