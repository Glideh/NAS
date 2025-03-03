# Misc Linux takeaways (OpenMediaVault / Debian based)

## Rsync

### Classic Archive Rsync command

rsync <--dry-run> -alv <source> <target>

--dry-run to try before exeture
-alv archive, links and verbose

https://explainshell.com/explain?cmd=rsync+-alv

trailing slash ARE significant : try before execute !

### Update Rsync

After copying (not using rsync) a bunch of files from a folder to another, rsync will try to copy again other them.

The right thing to do is add -u option as in: 

rsync <--dry-run> -aulv <source> <target>

https://explainshell.com/explain?cmd=rsync+-aulv

## Save energy with part-time hdd usage

(Tested with SATA disks / unsure with SAS)

OpenMediaVault (through hdparm) comes with device APM and spindown
While totally unpowered, disk is not consuming any energy
Of course, it's as slow as on boot to recover a fully spinning disk available for data r/w

Warning : Spindown may not be the better ways to save your disk health. Spin down a disk for a few minutes IS a bad idea. Use with caution :)
May be used with backup md devices, powered a few minutes a day / a week

In OMV ui : Storage > Disks > Edit

* Advanced Power Management
Set to '1 - Minimum power usage with standby (spindown)'

* Spindown time : in minutes before spindown disk

## Wake-On-Lan

Ethernet port led HAS to be lit to wake PC with a magic packet.

In order to keep ethernet device able to be awaken, issue this ethtool command : 
ethtool -s <device> wol g

Also, try suspending the PC instead of halting.

https://necromuralist.github.io/posts/enabling-wake-on-lan/
