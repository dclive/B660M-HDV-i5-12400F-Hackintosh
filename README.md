**Hackintosh EFI Information for Asrock B660M-HDV and i5-1200F - Fully working, OC83, MacOS12.5**.1 

*MAJOR CHANGES:  *

*Now works with B660M HDV BIOS 8.01 and Intel's Turbo works again for a significant speedup.  OpenCore .83.*  MacOS 13.0 Ventura B5 is lightly tested; Ventura works.  

 

![Screen Shot 12400](https://user-images.githubusercontent.com/4536776/151729828-5aa17320-ac7c-4992-802f-4a97c88f427e.png)

![183303929-75a8924a-09b9-4de0-acbd-d149a6264262](https://user-images.githubusercontent.com/4536776/183303929-75a8924a-09b9-4de0-acbd-d149a6264262.png)

**Credits**

Most content was sourced from https://github.com/Xmingbai/ASUS-TUF-GAMING-B660M-PLUS-Wi-Fi-D4-Hackintosh.  In order to facilitate the greatest number of people getting this information, I've slightly modified these files, tested it on the Asrock B660M-HDV, and am publishing the results in English.  If there are items I've missed (as again, this is not my original content), please file an issue so I can correct it.  Modifications are:  Kernel/Quirks/AppleXcpmForceBoost (for better turbo), networking, unselection of unused SSDTs, unselection of unused KEXTs, use of CorpNewt's https://github.com/corpnewt/CPU-Name CPU-Name to update the About-This-Mac information, and removal of the "ProvideCurrentCPUInfo" quirk so that Intel CPUs with no efficiency cores can boot properly (if you're using a 12th gen with efficiency cores, you can leave this enabled).  For an excellent discussion in English on these details and more, see https://www.tonymacx86.com/threads/z690-chipset-and-alder-lake-cpus.316618/ and https://www.tonymacx86.com/threads/asus-z690-proart-creator-wifi-thunderbolt-4-i7-12700k-amd-rx-6800-xt.318311/page-21#post-2304707.  

**Tested macOS**

* Monterey 12.5.1 with OC83/OC84 [The picture shows 12.2, which works fine too, but I focus on current MacOS releases in testing].    

**Hardware**

* Asrock B660M-HDV with BIOS 8.01 works well.  It's safe to update to 8.01, and all testing will only include 8.01 (or later) going forward.  Flash to 8.01.    
* Intel i5-12400F
* AMD RX 5700 GPU or AMD RX 6800XT GPU  [An AMD GPU is required regardless of which 12th gen CPU you use, no exceptions]
  * Most typical, RX470, RX480, RX570, RX580, RX590, Vega 56, Vega 64, RX 5700, RX6600, RX6600XT, RX6800, RX6800XT, RX6900XT will all work.  Some other variants (some RX560, for example) will work also, but you should google for more details before buying.  

* 64GB RAM PC3200 [2 x 32GB DIMMs]
* 2TB NVME [ADATA 8200 Pro]
* Corsair RM650x
* PowerMac G5 Case, LaserHive MATX 120 modifications
* BCM94360CS2 wifi card (https://www.amazon.com/dp/B01L6YWGXW) with M2 to NGFF adapter (https://www.ebay.com/itm/BCM94360CS2-Card-To-NGFF-M-2-Key-A-E-Adapter-For-Mac-OS-and-Hackintosh/391512537270?hash=item5b27f738b6:g:wIEAAOSw42JZGAtx) - fits perfectly on this motherboard in the wireless slot; no BIOS blocking or whitelisting of add-in cards exists on this machine.

**Working**

* Bluetooth, Wi-Fi [See above add-in card] and LOM ethernet
* AMD GPU HDMI & DP Audio;motherboard 3.5MM audio out (audio in is untested)
* Sleep / Wake works; mouse / keyboard wakes machine
* App Store, Time Machine [But to recover, keep track of your USB stick with your serials/MAC Address/etc. embedded in it!]
* Apple Watch unlock (mostly reliable, not perfect), AirDrop
* USB port mapping is complete, resulting in iPhone/iPads charging at 2100 ma, and Apple Watch at 1000 ma.  If yours (iPhone, iPad, Apple Watch) doesn't show this rate in About This Mac / System Report / USB, then your USB mapping may not be working correctly.  All 'A' USB ports on this motherboard, USB2 and USB3, are in use and 'active' (working).  USB-C port works, but is only lightly tested. 

**Untested**

* N/A

**Not Working**

* Broadly, anything requiring Intel QuickSync / Intel graphics won't work, since the Intel Xe graphics on 11th-12th gen isn't supported by MacOS in any capacity whatsoever - zero functionality, no exceptions.  
* Sidecar, as the Intel i5-12400F doesn't have an iGPU, and SideCar supports either the iGPU or a T2, not an AMD GPU.
* All video-out ports on the motherboard, as Intel 11th-12th gen (Xe) iGPU isn't supported in MacOS. 
* Universal Control and related functions don't work for me.  

**Disabled**

* In the Asrock B660M-HDV BIOS:  Disable:  Fast Boot, serial port, CSM, CAM (Clever Access Memory, AKA Resize Bar)

**Enabled**

* Above 4G decoding, VT-x, VT-d

**Next Steps - Required**

You will need to do the following: 

* Prepare a USB boot disk for MacOS 12.x installation.  The easiest way is on a real Mac, although gibMacOS may work for you as well.  To follow the much easier Real Mac path, read https://support.apple.com/en-us/HT201372 and follow the directions for MacOS, including the terminal command to write the download to the USB stick.  You'll want to format the USB as HFS+ format, GUID.  
* Download EFIAgent (https://github.com/headkaze/EFI-Agent) and mount the EFI (ESP) partition for the USB stick you just made.  Using EFIAgent again, "open" the EFI partition so it shows on the Mac desktop.  Note that EFI partitions are typically GRAY in color in EFIAgent.  To find EFIAgent, locate the new icon in the upper right clock area that looks like a circular pie.  ![Screen Shot 2021-09-25 at 7 22 44 PM](https://user-images.githubusercontent.com/4536776/134790066-27597b9e-a37f-47e0-87f5-d3ebbc2af59f.png)

 >>  Remember this process for any future EFI partitions you must mount; this is a common procedure.

* Copy the contents of the attached zipfile to the USB stick, so that your files look something like the picture: 

![EFI Layout](https://user-images.githubusercontent.com/4536776/134783624-10b0c7ba-fb29-4cf1-8017-230d22f8e18b.png)

* The EFI (ESP) partition on the USB stick has an EFI folder in it, in the root, and inside of that folder, there are two subfolders, OC and Boot, each with files in them.  Make sure your EFI partition looks just like this once you've unzipped the zipfile. 
* Note that you'll have TWO partitions on that one USB stick:  the EFS/EFI partition (which has just the EFI folder on it, with the contents above) and the other partition, usually called 'Install MacOS Monterey' which will house the Mac's OS/installation details.  

Technically, you are now done.  You should be able to boot MacOS using the USB stick, and install MacOS onto your SSD.  That said, I usually suggest configuring it a bit *after* you boot into MacOS for the first time with the right serials and ROM info: 

* Download OCAT https://github.com/ic005k/QtOpenCoreConfig and open it.  Read the tooltips showing what all the icons at the top do.  Update to the latest OCAT version by finding the update button and updating.  Don't continue until you've done this.  Run the latest OCAT version.  As of last edit, OC83 is current and fully working.  Over the course of time further updates will be required.
* Open your USB stick's config.plist by using OCAT's OPEN icon.
* In OCAT, notice the row of icons on the left side.  Go to "PI" on the row. 
* Let's generate a new serial.  Ensure, under the GENERIC tab, that for "SystemProductName" you have the MacPro7,1.  Then click GENERATE right next to the MacPro7,1 box.  Your serial numbers are now set up.  

Now let's fix your MAC address (ROM) 

* [Mac: This only works if you used the USB stick to install MacOS already; assumes MacOS is installed on this motherboard] Go to the Mac's terminal, type in "ifconfig", and find "en0:", your ethernet adapter.  Find the line "ether xx:xx:xx:xx:xx:xx" and write those numbers, without the :, into the ROM box.  
* [Using Windows, if Windows is installed on this motherboard] Go to Windows' commandline/powershell interface.  Type 'ipconfig /all' and find your ethernet adapter.  Find the line "Physical Address" with xx-xx-xx-xx-xx-xx letters to the right on that same line.  Key in those letters and numbers, without the -, in the ROM box. 
* [The easy way; untested] Click GENERATE immediately to the right of the ROM box. 
* Serialization and ROM setup is now complete.  Press the SAVE icon in OCAT and then quit OCAT. 
* Your USB stick is ready to use to boot your Mac and install MacOS.  
* **After all of the above:  Go to Dortania's page and read - before logging in with any AppleID accounts.  https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html**

**Final Steps**

* Assuming no other issues, your setup is now complete!   
* Restart, press F11 at the Asrock boot screen so you can choose a boot disk, and boot from the USB stick (select the uEFI option if prompted).  You'll then be able to step through installation of MacOS.  You'll need to format your SSD as APFS or HFS+ (APFS is the new one; use that).  Name the newly formatted SSD something like **MacHD** so you know that's what you'll boot from in the future.    
* Once setup is done, use EFIAgent to copy the USB stick's EFI folder, with your serial number modifications, to the SSD's EFI partition, and then you'll be able to boot from that disk (and you won't need the USB stick anymore, but keep it forever as a backup!). Do note:  Until you've copied the EFI folder from your USB stick to your SSD's EFI partition, you must continue to F11-boot into your USB stick before booting into MacOS.  Once you've copied the USB stick's EFI folder to the EFI partition on the SSD, then you'll no longer need to use the USB stick to boot.  
* Versioning on this zipfile is OC83.  Future versions, if required, would have higher numbers so it is easier to see what version you have.  Keep the zipfile (name, at least) around so you know what version you have.  
* You can clean up logs and logging / bootup, if you wish, once you have everything sorted.  Doritania's guide has a post-install cleanup section with good details on that.  In the zip, logging is fully enabled, so that if there's a problem you can take a video of the screen on your phone and troubleshoot based on that.
* Use OCAuxiliaryTools to update to later OpenCore releases, like OC84, which works fine.  Use MacOS's built-in update mechanism to update MacOS releases.  
* Otherwise, please leave comments/issues here. 
