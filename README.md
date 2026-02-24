# Ansible Collection - sde.h5

Documentation for the collection.

## Roles
get_ios_version - Collects information about IOS Version
get_run_config - Collects information about running configuration
get_start_config - Collects information about startup configuration
get_vlan_dat - Collects information about VLAN database
save_config - Saves the configuration collected to a file
update_ios - Updates the IOS image on the device

## Notes

### Init Ansible
```
sudo apt install ansible

# Creates folder structure for new collection
ansible-galaxy collection init namespace.collection

# Creates folder structure for new role
cd roles
ansible-galaxy roles init my_role
```

### Raspberry Pi Console Server

OS: Ubuntu Noble

guide used: https://www.jpaul.me/2019/01/how-to-build-a-raspberry-pi-serial-console-server-with-ser2net/
ser2net yaml: https://wifizoo.org/2023/05/12/yet-another-ser2net-tutorial/

```bash
# Install
sudo apt update
sudo apt upgrade -y
sudo apt install ser2net -y

# Find USB Serial Port (plug in USB to device to see them appear)
sysadmin@RPI-Console:~$ sudo su
root@RPI-Console:/home/sysadmin# dmesg | grep tty
[    0.000000] Kernel command line: coherent_pool=1M 8250.nr_uarts=1 snd_bcm2835.enable_headphones=0 snd_bcm2835.enable_headphones=1 snd_bcm2835.enable_hdmi=1 snd_bcm2835.enable_hdmi=0 snd_bcm2835.enable_hdmi=0  vc_mem.mem_base=0x3ec00000 vc_mem.mem_size=0x40000000  console=ttyS0,115200 multipath=off dwc_otg.lpm_enable=0 console=tty1 root=LABEL=writable rootfstype=ext4 rootwait fixrtc cfg80211.ieee80211_regdom=DK
[    0.000553] printk: legacy console [tty1] enabled
[    0.621782] printk: legacy console [ttyS0] disabled
[    0.625030] 3f215040.serial: ttyS0 at MMIO 0x3f215040 (irq = 71, base_baud = 50000000) is a 16550
[    0.629197] printk: legacy console [ttyS0] enabled
[    2.487002] 3f201000.serial: ttyAMA1 at MMIO 0x3f201000 (irq = 99, base_baud = 0) is a PL011 rev2
[    2.497567] serial serial0: tty port ttyAMA1 registered
[   11.854913] systemd[1]: Created slice system-serial\x2dgetty.slice - Slice /system/serial-getty.
[   11.943865] systemd[1]: Expecting device dev-ttyS0.device - /dev/ttyS0...
[ 1410.007720] usb 1-1.2: FTDI USB Serial Device converter now attached to ttyUSB0
[ 1435.559027] usb 1-1.3: FTDI USB Serial Device converter now attached to ttyUSB1
[ 1464.356587] usb 1-1.1.3: FTDI USB Serial Device converter now attached to ttyUSB2
[ 1472.548600] usb 1-1.1.2: FTDI USB Serial Device converter now attached to ttyUSB3

# Edit ser2net config file to add serial ports
sudo nano /etc/ser2net.yaml
# Example config for 4 serial ports
connection: &conUSB0
    accepter: telnet,tcp,3000
    enable: on
    options:
      banner: *banner
      kickolduser: true
      telnet-brk-on-sync: true
    connector: serialdev,
              /dev/ttyUSB0,
              9600n81,local

connection: &conUSB1
    accepter: telnet,tcp,3001
    enable: on
    options:
      banner: *banner
      kickolduser: true
      telnet-brk-on-sync: true
    connector: serialdev,
              /dev/ttyUSB1,
              9600n81,local

connection: &conUSB2
    accepter: telnet,tcp,3002
    enable: on
    options:
      banner: *banner
      kickolduser: true
      telnet-brk-on-sync: true
    connector: serialdev,
              /dev/ttyUSB2,
              9600n81,local

connection: &conUSB3
    accepter: telnet,tcp,3004
    enable: on
    options:
      banner: *banner
      kickolduser: true
      telnet-brk-on-sync: true
    connector: serialdev,
              /dev/ttyUSB3,
              9600n81,local

# Restart ser2net to apply changes
sudo systemctl restart ser2net

# Check status
sudo systemctl status ser2net

# Troubleshooting: address already in use error
sudo lsof -i :3000
# if ports are used, change accepter port in config file and restart ser2net

# Test on localhost
telnet localhost 4000

# Test from remote host
telnet <RPI_IP_ADDRESS> 4000

```

### Expect Shell Script

guides: 
    - https://community.cisco.com/t5/devnet-general-knowledge-base/network-automation-basics-automate-shell-scripts-with-expect/ta-p/4646253
    - https://www.geeksforgeeks.org/linux-unix/expect-command-in-linux-with-examples/

```bash

# Install expect
sudo apt install expect -y
sudo nano base_config.exp
chmod +x base_config.exp

```

