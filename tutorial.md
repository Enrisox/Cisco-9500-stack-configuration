# Cisco Catalyst 9500 StackWise Virtual – Two-Switch Setup

This guide describes how to upgrade the firmware on a Cisco Catalyst 9500 switch, prepare both devices, and create a StackWise Virtual pair between two switches.

![the 2 switches](./img/BD82E47A-5818-495E-B714-DF8971BDD16B.jpg)

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

On **Switch 1** and switch2:
```bash
configure terminal
interface TenGigabitEthernet1/0/38
stackwise-virtual dual-active-detection
exit
write memory
reload
```
After both switches reload, verify DAD status:
```bash
show stackwise-virtual dual-active-detection
```

You should see interfaces `TenGigabitEthernet1/0/38` and `TenGigabitEthernet2/0/38` in **up** state (one per switch).

### Upgrading the Stack Software (Both Switches)

Once the stack is formed, you can upgrade the software on all members by copying the new `.bin` image to the `bootflash:` of **one** switch.

1. Copy the desired `.bin` image to `bootflash:` on one of the switches.  
2. From the active switch, run:
```bash
request platform software package install switch all file bootflash:<image-name>.bin auto-copy new
```

3. Wait for the installation to complete and for the stack to reload if required.
4. Verify the software version on both switches:
5. Check the StackWise Virtual status:

```bash
show version
show stackwise-virtual
show ip interface brief
```

Make sure that `Te1/0/39`, `Te1/0/40`, and the corresponding ports on the second switch are in **up/up** state, confirming that the StackWise Virtual Links are operational.

