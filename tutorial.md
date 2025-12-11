# Cisco Catalyst 9500 StackWise Virtual – Two-Switch Setup

This guide describes how to upgrade the firmware on a Cisco Catalyst 9500 switch, prepare both devices, and create a StackWise Virtual pair between two switches.

## Firmware installation from USB

1. Copy the new Cisco IOS XE firmware image to a USB drive.
2. Insert the USB drive into the switch.
3. Verify that the switch detects the USB and check the exact image name:

```bash
dir usbflash0:
```
4. Install the new firmware image (example for switch 1, adjust filename as needed):
```bash
request platform software package install switch 1 file bootflash:<image-name>.bin
```
5. Reload the switch:
```bash
reload
```
6. Skip the **initial configuration dialog** when prompted.
7. After the reboot, verify the running firmware version:


> Note: When you power on the switch for the first time, you can let the automatic installation run so that the management port obtains an IP address via DHCP.  
> This can be useful if you want to download the firmware image directly from the internet instead of using a USB drive.

## Creating a StackWise Virtual pair (two Cisco Catalyst 9500)

The two switches must:
- Be the same Cisco Catalyst 9500 model.
- Run the same IOS XE software version.
- Have compatible licenses for StackWise Virtual.

### Configure StackWise Virtual on Switch 1

1. Enter global configuration mode:
2. Enable StackWise Virtual and set the domain ID:

```bash
configure terminal
stackwise-virtual
domain 1
exit
write memory
reload
```


### Configure StackWise Virtual on Switch 2

Repeat the same steps on the second switch:


### Configuring StackWise Virtual Links (SVL)

On **Switch 1**:

1. Enter configuration mode:
2. Configure the StackWise Virtual Links on interfaces TenGigabitEthernet1/0/39 and 1/0/40:
3. Exit configuration mode and save:
4. Reload the switch:

```bash
conf t
interface TenGigabitEthernet1/0/39
stackwise-virtual link 1
exit
interface TenGigabitEthernet1/0/40
stackwise-virtual link 1
exit
write memory
```

Repeat the same configuration on **Switch 2** (adjusting the slot/port notation if needed).

### Configuring Dual-Active Detection (DAD)

Use a third physical link between the two switches for split‑brain protection (Dual‑Active Detection), for example on interface TenGigabitEthernet1/0/38 on both switches.

On **Switch 1**:




