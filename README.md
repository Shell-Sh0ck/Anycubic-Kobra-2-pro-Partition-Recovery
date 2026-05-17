# Anycubic-Kobra-2-pro-Partition-Recovery

Ways to backup partitions on **Trigoilla_Spe_B_V1.0.x** motherboards using ssh or (xrock, upgrade tool):
**All backup partitions for restoring the operating system only will be included in the releases.**
1. ssh(root@192.168.1.77)
   The ssh method only works if your device is booting, connected to your AP (Access point WiFi) and has rinkhals installed on it, since ssh is not pre-installed in the regular  firmware.
   Now, to connect to the 3d printer, you need to find out its IP address on the local network. To do this, you can go to the printer settings and look there.: **Settings** -> **More Settings** -> **About Machine**. The WiFi IP field will specify the IP of your 3d printer.
<img width="827" height="1280" alt="5211226516690247494" src="https://github.com/user-attachments/assets/c89eedec-e1ce-4c1f-aa3d-394eeb795808" />
But if you can't find out the ip of your device in this way, you can scan your local network and find the ip of your device: **nmap -n -sV -p 20 <ip you router>/24**
When you have found out the ip address of your printer, we connect to it using ssh: ssh root@192.168.1.77 password **rockchip**
<img width="913" height="118" alt="image" src="https://github.com/user-attachments/assets/581134f6-7871-4722-88dc-48855a5dcd8d" />

**WARNING**: The script will prompt you to clear sensitive data, agree if you are going to publish it, it will delete credentials, certificates but will not affect **wifi_cfg**. Also, before executing the script, make sure that your flash drive is formatted in **exFAT(fat64)**, if it is not converted to the desired format, the error dd: file to large will appear and your partitions will be corrupted. **The author of the scripts, that is, I do not bear any responsibility for your actions!**

After connecting, copy the script to **/root/backup_fwdd.sh**: https://gist.github.com/Shell-Sh0ck/3ee691212681474f65ad69851bf7e6ab#file-backup_fwdd-sh creating
nano **backup_fwdd.sh** and insert the script. Making it executable: **chmod +x ./backup_fwdd.sh** and launch it **./backup_fwdd.sh** Be sure to insert a flash drive of at **least 10 gigabytes** in size

<img width="1041" height="1413" alt="image" src="https://github.com/user-attachments/assets/834fba99-b7ef-41ac-ac1d-f01b8c3bbb79" />
Now, after the script is completed, all the sections of the 3d printer are on the flash drive. (Optional) You can mount userdata and useremain to correct their contents (the same wifi_cfg):
```
mount -o loop,rw ./useremain /mnt/useremain 
mount -o loop,rw ./userdata /mnt/userdata
```
**All backup partitions for restoring the operating system only will be included in the releases.**
2. (xrock, upgrade tool)
   In this method, you will have to open the printer and take out the motherboard to solder the usb (OTG) pins on the back of the motherboard as described in the above discussion: **https://github.com/Bushmills/Anycubic-Kobra-3-rooted/discussions/5** this only works with **Trigoilla_Spe_B** motherboards.
   
   **Motherboard: Trigoilla_Spe_B_V1.0.5**
   <img width="720" height="1280" alt="Untitled3" src="https://github.com/user-attachments/assets/98e64da2-4964-47ee-8e43-4e901fe0d960" />
   <img width="493" height="321" alt="397723434-e75bfe6f-ec81-43cc-bfe9-87b0ffc491d4" src="https://github.com/user-attachments/assets/12370b0d-88c4-46b9-adb3-212256e17e76" />
   <img width="720" height="1280" alt="Untitled" src="https://github.com/user-attachments/assets/e857d5cc-18ca-4809-ac32-e4e12e251b7e" />
   
   **Motherboard: Trigoilla_Spe_B_V1.1.5**
   <img width="3024" height="4032" alt="377813827-4da806d9-014f-4139-8c9d-cef4a0998e15" src="https://github.com/user-attachments/assets/6dda20ff-9351-414e-ba0a-9b237ed9b075" />
   <img width="4000" height="3000" alt="361245682-56d29dc4-ce3e-482c-8ff0-d4d320daf5e0" src="https://github.com/user-attachments/assets/9e5e00da-028f-46c1-b92d-1b5298618435" />
After you solder into the **5v,D+,D-,GND** pins, be sure to disconnect the power supply from the motherboard, the board must be powered by USB.
Install xrock(https://github.com/xboot/xrock ) and upgrade tool(https://docs.radxa.com/en/som/cm/cm3j/low-level-dev/upgrade-tool)
Hold down the sw2 button on the motherboard and insert the USB without releasing it, wait for 5 seconds, then you can release and perform lsusb.
<img width="658" height="252" alt="2026-05-07_23-02" src="https://github.com/user-attachments/assets/f105f5c9-3299-41ad-a68c-c93c8be50120" />
Next, go to the xrock tool directory and run the command: ```sudo ./xrock maskrom rv1106_ddr_924MHz_v1.15.bin rv1106_usbplug_v1.09.bin --rc4-off``` next, go to the upgrade tool directory and execute ```./upgrade_tool pl``` copy part of the output to ```nano ./list.txt```
<img width="957" height="346" alt="image" src="https://github.com/user-attachments/assets/cdab1b1c-e263-4e33-b8da-b4a87d31f243" />
As you performed the steps above, copy the script to the directory with the **upgrade_tool** tool https://gist.github.com/Shell-Sh0ck/3ee691212681474f65ad69851bf7e6ab#file-backup_firmware-sh and make it executable: ```chmod +x ./backup_firmware.sh``` . Next, execute it ```./backup_firmware.sh```
All sections will be placed in the **output** directory.

How to flash partitions on **Trigoilla_Spe_B_V1.0.x** motherboards using (xrock, upgrade_tool).
1. At this stage, I think you already have (xrock, update_tool), so to flash the partitions, also copy this script https://gist.github.com/Shell-Sh0ck/3ee691212681474f65ad69851bf7e6ab#file-update_firmware-sh in the update_tool directory, change the path in the variable ```firmware_path="/home/motherfucker/Desktop/parts_backup-k2p"``` to specify where your partitions are stored. Make the script executable by ```chmod +x ./update_firmware.sh``` and fulfill it ```./update_firmware.sh```
<img width="660" height="879" alt="2026-05-08_00-03" src="https://github.com/user-attachments/assets/83fcee77-75fd-49ab-83d4-be0afa5be64e" />

**If you want to add or change something, please create issues or a pull request.**

