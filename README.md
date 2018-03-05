# huawei_quirks

Huawei does weird things.

# boot
kernel, ramdisk and recovery are split and stored as Android bootimg ( ANDROID! magic )
* kernel = kernel and cmdline
* ramdisk = android ramdisk ( cpio archive )
* ramdisk_recovery = recovery ramdisk ( cpio archive )

# fastboot
Sometimes during your development journey you'll get stuck in reboot loop and holding volume down won't get you in fastboot. Don't panic, the easiest way to get back there is to boot to eRecovery ( hold volume up for 3 seconds while 1,2,3 screen is visible ) then power-off your phone and plug in the USB cable while holding volume down ( * woop woop * anyone remembers MTK flash tool? )

# vbmeta
Yep, also that, huawei is using a custom implementation not supported by AOSP  
They are using 2 different dts entries ( vbmeta_huawei and vbmeta_hisi ) while aosp expects only vbmeta ( HACK: Merge them together )  
Oh also they set by_name_prefix at runtime :D  

# fstab ( early mount)
Same as vbmeta, they format the dev string at runtime ( /dev/**/bootdevice + actual dev entry )

-> /dev/block/platform/ff3c0000.ufs/ is the only possible dev path on this device, so we can hack DTS

# modem
/sys/devices/platform/his_modem/modem_sysboot_start you can guess what it does ( handled in /init althought people tend to set ril-daemon to disabled and start ril-daemon manually after writing 1 to the modem sysfs node, see: [this](https://github.com/search?utf8=%E2%9C%93&q=modem_sysboot_start+filename%3A*.rc&type=) )

# mounted partitions
| Dev block | Mount Point | FS Type | Flags |
| ----------|-------------|---------|-------|
/dev/block/dm-0 | /system | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-1 | /vendor | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-2 | /odm | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-3 | /version | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-4 | /product | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-5 | /cust | ext4 | (ro,seclabel,relatime,data=ordered)  
/dev/block/sdd7 | /sec_storage | ext4 |  (rw,context=u:object_r:teecd_data_file:s0,nosuid,nodev,noatime,discard,nodelalloc,data=journal)  
/dev/block/sdd17 | /splash2 | ext4 | (rw,context=u:object_r:splash2_data_file:s0,nosuid,nodev,noatime,data=ordered)  
/dev/block/sdd60 | /data | f2fs | (rw,seclabel,nosuid,nodev,noatime,background_gc=on,discard,user_xattr,inline_xattr,acl,inline_data,inline_dentry,extent_cache,mode=adaptive,verify_encrypt,inline_encrypt,active_logs=6)  
/dev/block/sdd58 | /cache | ext4 | (rw,seclabel,nosuid,nodev,noatime,data=ordered)  
/dev/block/sdd27 | /hisee_fs | ext4 | (ro,context=u:object_r:hisee_data_file:s0,relatime,data=ordered)  
/dev/block/sdd3 | /modem_secure | ext4 | (rw,context=u:object_r:modem_secure_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered)  
/dev/block/sdd45 | /modem_fw | ext4 | (ro,context=u:object_r:modem_fw_file:s0,relatime,data=ordered)  
/dev/block/sdd11 | /mnvm2:0 | ext4 | (rw,context=u:object_r:modem_nv_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered) 
/dev/block/sdd8 | /modem_log | ext4 | (rw,context=u:object_r:modem_log_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered)  
