Configure a 3.5inch display (XPT2046 touch controller)
======================================================

To get my display working I followed [a post from retropie.org.uk forum][1]. [Once I had Retropie installed installed in my SD card][2], I followed these steps:

1. Plug my TFT and boot up RPi.
2. Once Retropie is up and running, [connect to Rpi using ssh][3]:
  ```
  ssh pi@192.168.1.xxx

  (If you haven't changed it, pass should be 'raspberry')
  ```
3. Get the proper overlay:

  ```
  pi@retropie:~/tft $ git clone https://github.com/swkim01/waveshare-dtoverlays.git
  ```
4. As I am running Retropie with a 4.4 Kernel (`uname -a` to check it out) I copied the following file to `/boot/overlays`:

  ```
  pi@retropie:~/tft $ sudo cp waveshare-dtoverlays/waveshare35a-overlay.dtb /boot/overlays/waveshare35a.dtbo
  ```
5. Edit `/boot/config.txt` and add these to lines at the end:

  ```
  dtparam=spi=on
  dtoverlay=waveshare35a
  ```
6. Reboot RPi:

  ```
  sudo reboot
  ```
7. Once reboot is finished, connect to RPi using ssh again and execute `ls /dev/fb*`. You should see `/dev/fb0` and `/dev/fb1`. The latter (fb1) is our TFT.
8. Now we need to copy output to our TFT. We need to install `cmake` first:

  ```
  sudo apt-get install cmake
  ```
9. Clone the utility to perform the copy:

  ```
  pi@retropie:~/tft $ git clone https://github.com/tasanakorn/rpi-fbcp
  ```
10. Use `cmake` to build the project:

  ```
  pi@retropie:~/tft $ cd rpi-fbcp/
  pi@retropie:~/tft/rpi-fbcp $ mkdir build
  pi@retropie:~/tft/rpi-fbcp $ cd build/
  pi@retropie:~/tft/rpi-fbcp/build $ cmake ..
  ...
  -- Build files have been written to: /home/pi/tft/rpi-fbcp/build
  pi@retropie:~/tft/rpi-fbcp/build $ make
  ...
  [100%] Built target fbcp
  ```
11. Install `fbcp`:

  ```
  pi@retropie:~/tft/rpi-fbcp/build $ sudo install fbcp /usr/local/bin/fbcp
  ```
12. Edit `/etc/rc.local` to launch `fbcp` automatically. Before the last line (`exit 0`), add:

  ```
  /usr/local/bin/fbcp &
  ```
13. Reboot Rpi: `sudo reboot`

[1]: https://retropie.org.uk/forum/topic/295/retropie-and-waveshare-32b/2
[2]: ./installing-retropie.md
[3]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
