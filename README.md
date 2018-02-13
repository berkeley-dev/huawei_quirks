# huawei_quirks

Huawei does weird things.

# boot
kernel, ramdisk and recovery are split and stored as Android bootimg ( ANDROID! magic )
* kernel = kernel and cmdline
* ramdisk = android ramdisk ( cpio archive )
* ramdisk_recovery = recovery ramdisk ( cpio archive )

# vbmeta
Yep, also that, huawei is using a custom implementation not supported by AOSP  
They are using 2 different dts entries ( vbmeta_huawei and vbmeta_hisi ) while aosp expects only vbmeta ( HACK: Merge them together )  
Oh also they set by_name_prefix at runtime :D  

# fstab ( early mount)
Same as vbmeta, they format the dev string at runtime ( /dev/**/bootdevice + actual dev entry )

# modem
/sys/devices/platform/his_modem/modem_sysboot_start you can guess what it does ( handled in /init althought people tend to set ril-daemon to disabled and start ril-daemon manually after writing 1 to the modem sysfs node, see: [this](https://github.com/search?utf8=%E2%9C%93&q=modem_sysboot_start+filename%3A*.rc&type=) )
