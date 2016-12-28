# Installing Retropie Image

To copy Retropie image to my SD card on Linux I followed the instructions provided by [the Raspberry Pi Foundation][1].

1. Run `df -h` command before inserting your SD card and then run it after inserting the card to find out which device is your SD card (it'll be the device that appeared the second time you ran `df -h` command). E.g., in my case it was listed as `/dev/sdc1` and `/dev/sdc2` as my card had two partitions from a previous Retropie installation.

 > __NOTE__: if your card is listed as something like `/dev/mmcblk0p1`, then `p1` is the partition number, not only `1`as in my case.

2. Unmount SD card (all partitions):
```
umount /dev/sdc1
umount /dev/sdc2
```
You can use `df -h` command again to check you correctly unmount the card.

3. Write the image to the SD card:
```
sudo dd bs=4M if=retropie-4.1-rpi1_zero.img of=/dev/sdc
```
Note that you must remove partition number from your device name. If `bs=4M` does not work, `bs=1M` can be used instead (it will slow down the process, but should work)

  `dd`command does not provide progress info. You can check [the original instructions I followed][1] to know how to see progress info and also how to check the image was correctly written to your card.

4. Run `sudo sync` command to ensure the write cache is flushed and you can safely unmount your SD card.

---
This document uses content from the [Raspberry Pi Foundation page][1], that also uses content from the eLinux wiki page [RPi_Easy_SD_Card_Setup][2].

[![CC BY-SA][3]](https://www.raspberrypi.org/creative-commons/)

[1]: https://www.raspberrypi.org/documentation/installation/installing-images/linux.md "Raspberry Pi Foundation"
[2]: http://elinux.org/RPi_Easy_SD_Card_Setup
[3]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
