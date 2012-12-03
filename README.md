This is a powersave script for Linux. Initially, I've created it for my own laptop. That means that this script will **not** work on any setup so please **check** this before running it on your computer! Things might break otherwise and I will **not** be responsible for any damage that has been done. This script gives me roughly 1:30/2:00 hours of battery life, on a battery that would otherwise last ~45 minutes. (yea, I know, my battery is fried)

The script will need the following dependencies to run:
* hdparm
* wireless_tools
* xset
* udev
* udisks

This script has to be placed in `/usr/bin` and when done so, one can run `powersave true|false` and the powersaving will kick in. I have also made a udev rule that will automatically run these commands depending on battery state (Charging, discharging). This rule will also enable|disable CD polling by Udisks.

Things you should edit, should you use this on your setup:
* NMI watchdog; I have this disabled at kernel level. If you don't, then uncomment the line in the powersave script. This option is safe to do! Should you want to disable this at kernel level too, use: `nmi_watchdog=0`
* ASPM powersave; this option is safe! To be able to write to this file you have to use a kernel parameter at boot: `pcie_aspm=force`
* Disk powersave; as I have only one HDD (laptop), I have edited the wildcard that Taylorchu originally uses. If you have multiple HDD's and want to run powersave on all of them, then use this:
  `for dev in $(awk '/^\/dev\/sd/ {print $1}' /etc/mtab); do hdparm -S 6 -B 254 -a 2048 $dev; done`  
Also, note that I use -B254. I hate the clicking noises my HDD would make otherwise, but this is not power-efficient. If you want maximum powersave, use -B1. **THIS CAN HURT YOUR HARDDRIVE, SO BE CAREFUL.**
* Screen powersave; this option is safe! The numbers of brightness differ per manufacturer. You should check yours. Do so by setting your screen on maximum brightness and then running `cat/sys/class/backlight/acpi_video*/brightness`
Then, lookup the `false)` part in the script and change the value. Mine is set to 9.

ToDo:
* Provide an easy way to blacklist modules
	/etc/modprobe.d/blacklist.conf
