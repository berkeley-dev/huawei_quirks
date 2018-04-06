mkbootfs ramdisk | minigzip -c -9 > ramdisk.gz
mkbootimg --kernel /dev/null --ramdisk ramdisk.gz --cmdline buildvariant=user --base 0x10000000 --pagesize 2048 --output ramdisk.img

mkbootimg --kernel Image.gz --ramdisk /dev/null --cmdline "loglevel=4 initcall_debug=n page_tracker=on unmovable_isolate1=2:192M,3:224M,4:256M printktimer=0xfff0a000,0x534,0x538 androidboot.selinux=enforcing buildvariant=user" --base 0x78000 --pagesize 2048 --kernel_offset 0x07b88000 --second_offset 0x00e88000 --tags_offset 0x07988000 --os_version 8.0.0 --os_patch_level 2018-01-01 --output kernel.img
