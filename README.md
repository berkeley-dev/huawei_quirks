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

# mounted partitions
/dev/block/dm-0 on /system type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-1 on /vendor type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-2 on /odm type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-3 on /version type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-4 on /product type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/dm-5 on /cust type ext4 (ro,seclabel,relatime,data=ordered)  
/dev/block/sdd7 on /sec_storage type ext4 (rw,context=u:object_r:teecd_data_file:s0,nosuid,nodev,noatime,discard,nodelalloc,data=journal)  
/dev/block/sdd17 on /splash2 type ext4 (rw,context=u:object_r:splash2_data_file:s0,nosuid,nodev,noatime,data=ordered)  
/dev/block/sdd60 on /data type f2fs (rw,seclabel,nosuid,nodev,noatime,background_gc=on,discard,user_xattr,inline_xattr,acl,inline_data,inline_dentry,extent_cache,mode=adaptive,verify_encrypt,inline_encrypt,active_logs=6)  
/dev/block/sdd58 on /cache type ext4 (rw,seclabel,nosuid,nodev,noatime,data=ordered)  
/dev/block/sdd27 on /hisee_fs type ext4 (ro,context=u:object_r:hisee_data_file:s0,relatime,data=ordered)  
/dev/block/sdd3 on /modem_secure type ext4 (rw,context=u:object_r:modem_secure_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered)  
/dev/block/sdd45 on /modem_fw type ext4 (ro,context=u:object_r:modem_fw_file:s0,relatime,data=ordered)  
/dev/block/sdd11 on /mnvm2:0 type ext4 (rw,context=u:object_r:modem_nv_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered)  
/dev/block/sdd8 on /modem_log type ext4 (rw,context=u:object_r:modem_log_file:s0,nosuid,nodev,noatime,noauto_da_alloc,data=ordered)  
