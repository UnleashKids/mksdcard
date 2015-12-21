###XO1 Cookbook
------------
* Install stock 13.2.5 from OLPC (9:10am) This involves unzipping the image to an USB stick and doing 4 button install. (see http://wiki.laptop.org/go/Release_notes/13.2.5XO-1_with_SD_card)
* Get the new SD card image booted, and on the network 
* Install git--"yum install -y git" (9:25am)
* Git clone https://github.com/georgejhunt/mksdcard
* Execute mksdcard/scripts/onxo1.0.sh (9:45am) 
* This script will reboot very quickly to change the size of temp file systems.
* Just su to root, and restart the script 
* done (11:15am)
* A reboot is required for all of the setup to complete.

As an aside if preparing to make a new distribution, do the following::

1. Issue the "xs-betaprep".
2. Put the SD card in another machine running linux
3. On that machine "git clone https://github.com/georgejhunt/mksdcard"
4. The script to execute is mkxscard/scripts/shrink_olpc.sh (this script defaults to schrinking /dev/sdb. If you are using an XO, change line 4 to "DEVICE=/dev/sda" (the USB SD card reader's device is usually /dev/sd<something>)
5. The script will shrink the second partition, dd it to a file, give that fine the name you specify, and zip it up for easy uploading and downloading.

###XO1.5 Cookbook
--------------
* On a linux machine, "git clone https://github.com/georgejhunt/os-builer --branch sdcard"
* Run the os-builder: "os-builder /root/mksdcard/olpc-os-xo1.5.ini"(5:44 --takes about 30min)

###XO4 Cookbook
I may need to add "iw dev wlan0 set 4addr" to rc.local in order for bridging to work.  But it seems maybe to persist through a reboot. (But has twice been required to get bridging configured -- and it seems so non-intuitive.)

##History
* The olpc-os-builder ini file I was able to craft did not transfer corectly to Gnome. So I shifted to the strategy of loading James Cameron's 13.2.5 image onto an SD card, and then modifying it with a bash script from there (making mkxo10.sh obsolete).
mkxo10.sh -- script which accepts an os-builder image name (assumed to be in /root/olcp-os-builder/images/xo1/). Then it partitions an SD card, mounts it, and copies the lzma tarball. Other parts of this script appear to be oblolete (as is almost all of it -- since I've decided to start with a stock 13.2.5 olpc build)

8gb -- this script tries to copy desired kiwix content to the SD card -- assumes that the content is on the same machine located at /root/content/zims

xo-custom -- written by Jerry Vonau, incorporated into mktinycorexo by James Cameron, perhaps useful in doing a chroot, and all of the assembly on a SD card on a larger machine.

onxo10.sh -- originally written for the xo1, to implement the additional functions (openvpn, sshd, kiwix, maybe apache). But then I tested it on the XO1.5, and added the phrase to download and install my custom kernel (which includes the bridge module needed for wifi networking). The obvious next step is break up onxo10 into onxo1.0.sh, and onxo1.5.sh.

XO1.5
=====
The preference is to use OS builder to generate the base image (if gnome works on the 1.5) because I can disable the funny chroot/file layout required by Quanta, and the update in the field strategy.

Three times, I've encoundered pip errors during loader process regarding __dso_handle. This time I yum removed gcc-4.7, and "yum install compat-gcc-43".
