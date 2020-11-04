## Preparation
- currently the 32-bit version is required because essential packages are not available, especially wiringpi
- download [Raspberry Pi OS (32-bit) Lite](https://downloads.raspberrypi.org/raspios_lite_armhf_latest)
- unpack *.zip
- flash *.img to SD card using Win32DiskImager

## Headless boot
### activate sshd
- after the image is flashed, the boot partition is visible under Windows (this is the /boot partition
- copy a file with name `ssh` to this partition, the content does not matter

### activate wifi
create file `wpa_supplicant.conf` in the /boot partition:

    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    country=DE
    
    network={
      ssid="<ssid>"
      psk="<password>"
    }

## Initial setup

Standard configuration
- hostname: raspberrypi
- username: pi
- password: raspberry

Initial remote ssh login with user pi:

    putty -ssh -pw raspberry pi@raspberrypi

Proceed with setup as root: `sudo -i`

Change password for user pi as root, otherwise passwd will reject password if it is not suitably complex:

    passwd pi

Enable ssh root login with public key:

    vi /etc/ssh/sshd_config
    uncomment line: `#PermitRootLogin prohibit-password`

Copy public key

    mkdir .ssh
    vi .ssh/authorized_keys
    # copy/past content from other machine
    chmod 600 .ssh/authorized_keys

Change hostname

    echo pi4 > /etc/hostname
    sed -i "s/127.0.1.1.*raspberrypi/127.0.1.1\tpi4/g" /etc/hosts

Set timezone:

    rm /etc/localtime
    echo "Europe/Berlin" > /etc/timezone
    dpkg-reconfigure -f noninteractive tzdata
    # alternatively: dpkg-reconfigure tzdata

Customize /boot/config.txt:
- I2C is needed, uncomment `#dtparam=i2c_arm=on` &rarr; `dtparam=i2c_arm=on`
- if Wifi is not needed, add `dtoverlay=disable-wifi` at the end
- Bluetooth is not needed, add `dtoverlay=disable-bt` at the end
- Audio is not needed, comment `dtparam=audio=on` &rarr; `#dtparam=audio=on`

System update:

    apt upgrade && apt update

Now reboot machine and connect with putty: `putty -load lokal root@pi4`
