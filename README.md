## Install any OS onto your Raspberry Pi

This guide is for people who uses a MacOS or Linux as their operating system, because I will be using terminals to explain. This guide will teach you how to install **ANY IMAGE FILE (Operating System)** onto an SD Card which then you can insert it into your Raspberry Pi and boot up the Operating system. 

### What you'll need
* Raspberry Pi Zero / 2 / 3
* Micro SD Card [4GB+ Preferably] with the adapter 
* A laptop or a desktop with MacOS or Linux

### STEP 1 - CONNECT YOUR SD CARD TO YOUR LAPTOP / DESKTOP 

Plugin your Micro SD Card to the adapter and the adapter to your laptop / desktop. This will bring up the device on the screen. 


### STEP 2 - TERMINAL 

Open up the terminal on your mac / linux Operating System. If you type in: 

```bash 
diskutil list
```
You can see the list of all the volumes that are connected to your computer. 
Now, by using the `diskutil` command we can *format, partition and also unmount* specific disks. 
On the list you will see something like this: 
```bash 
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Mac HD                  250.1 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                            Mac HD                 +249.8 GB   disk1
                                 Logical Volume on disk0s2
                                 xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
                                 Unlocked Encrypted
```
Now when you connect a new device and run the same command again, you will see there are more things that are added to the list. 
I just connected my SD card right now, and I got this: 
```bash 
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Mac HD                  250.1 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                            Mac HD                 +249.8 GB   disk1
                                 Logical Volume on disk0s2
                                 xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
                                 Unlocked Encrypted

/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *2.0 GB     disk2
   1:             Windows_FAT_32 NO NAME                 536.9 MB   disk2s1
   2:                      Linux                         1.4 GB     disk2s2
   ```
 Now you can see I have inserted a device `/dev/disk2` to the computer. 

#### STEP 2.1 - FORMATTING THE SD CARD
* Identify the disk that the SD card is connected to, for me its the `/dev/disk2`
* Enter the following command to format the SD card: 
 
```bash
diskutil eraseDisk ExFAT BOOT /dev/disk<number>
```

* The way it works is `diskutil eraseDisk <format-type> <name-of-volume> <identifier>`
* This will take some time depending on your SD card size and the contents present in it

#### STEP 2.2 - IMAGE FILE

I assume you have an image file of the Operating System already downloaded in your computer, If not you can select some of these, I have selected some of the most popular Operating Systems: 
* [Rasbian](https://downloads.raspberrypi.org/raspbian_latest)
* [Ubuntu](https://ubuntu-mate.org/raspberry-pi/ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img.xz.torrent)
* [RetroPi](https://github.com/RetroPie/RetroPie-Setup/releases/download/4.2/retropie-4.2-rpi2_rpi3.img.gz)
* [Kali Linux](https://images.offensive-security.com/arm-images/kali-2017.01-rpi2.img.xz)

Most of the links provided here are compressed, I am assuming you know how to decompress these files. 
After you decompress these files you will get the image file of the corresponding OS. 

#### STEP 2.3 - INSTALLING THE IMAGE FILE ONTO THE SD CARD 

* This is fairly simple, we have to use a command called `dd` on the terminal to do so.  This command looks somthing like: 
```bash
sudo dd bs=1M if=<image-file> of=<identifier-of-your-RAW_disk>
```
* I'll make it simpler, say you have downloaded the image file onto your desktop which has the location `/Users/username/Desktop/os.img` and your disk is `/dev/disk2`, you can recheck where your disk is located by entering the command: `diskutil list`. Now the command becomes
```bash
sudo dd bs=1M if=/Users/username/Desktop/os.img of=/dev/rdisk2
```
* Notice that you **HAVE TO ADD 'r' BEFORE YOUR DISK**, to mention that it's a raw disk. **NOT `/dev/disk2` BUT `/dev/rdisk2`**
* Now press enter, You might have to enter your password. Now, wait for the image file to be copied to the disk, this will take quite a while depending on the size of the image file and also on the write speed of your Micro SD card. 
* After this is done, You can insert the Micro SD card into your Raspberry Pi and power it up, It will automatically boot into the operating system that you have installed. 
