# How to setup device files for reading heads

file /etc/udev/rules.d/20-myUSBDevices.rules

    # udevadm info --attribute-walk --path=/sys/bus/usb-serial/devices/ttyUSB0
    # systemctl restart systemd-udevd.service
    # reicht nicht, wahrscheinlich reboot notwendig
    # SUBSYSTEM=="usb-serial"
    # SYMLINK+= oder NAME=
    
    # USB-IR-Schreib-Lesekopf 1
    SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", ATTRS{serial}=="0069EFD5", SYMLINK+="ttyUSB_Lesekopf1"
    # USB-IR-Schreib-Lesekopf 2
    SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", ATTRS{serial}=="0069FE04", SYMLINK+="ttyUSB_Lesekopf2"
