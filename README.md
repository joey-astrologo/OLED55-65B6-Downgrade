# OLED55-65B6-Downgrade
This firmware downgrader was not made by me. This repo is a compilation of information and software needed to downgrade to 4.31.20 for the B6 series OLED tv from LG.

The software can be used to downgrade to any firmware you may have, but only 4.31.20 for North American models is included.

# Instructions

1. Go to dns/AcrylicHosts.txt and edit the file with the ip address of the computer running the program. 
   The three line you need to edit are
   ```
   192.168.50.54 snu.lge.com
   192.168.50.54 lgtvonline.lge.com
   192.168.50.54 su.lge.com 
   ```
   Replace 192.168.50.54 with the ip address of your computer.
2. Make sure your tv and computer are plugged into the same router with ethernet cables.
   Got to the network settings on your tv and manually enter the network settings for wired connection.
   If your internet gateway is 192.168.50.1 then your settings should be;
   ```
   IP Address: 192.168.50.??? where ??? is a number not used by another device on your router
   SUBNET    : 255.255.255.0
   Gateway   : 192.168.50.1
   DNS SERVER: 192.168.50.??? where ??? is the ip address you entered in step 1
   ```
3. Download the firmware 

   * [Mirror 1](https://mega.nz/#!b0g3HaCK!1SAhfYg5n5Gpuuap1lNNS01w2wKexFNQ_ps97mTFZqs)
   * [Mirror 2](https://drive.google.com/open?id=0ByCYX-u487QXY1Q0SHN0ekU3SlE)

   Extract and copy the firmware to "C:\LgDTVUpDater\htdocs"

**The following steps are optional if you just want to downgrade to the included 4.31.20 firmware with a North American B6.**
   
4. Optional(already done) 
   We need to create the reponse file to the tv's firmware upgrade request.
   Run startDNS.bat and startHTTP.bat
   Open up the htdocs/in_dump.txt file
   Copy <MODEL_NM> field and rename the htdocs/HE_DTV_W16K_AFADATAA file with the field you copied
5. Optional(already done)
   Next we need to edit the file htdocs/"<MODEL_NM> field" from step 4
   
   * <IMAGE_SIZE> - should match the exact file size of the firmware. You can find it by right-clicking the firmware file and choosing      Properties. Copy the bytes value like in this picture:Remove the spaces after pasting it into <IMAGE_SIZE>
   * <IMAGE_NAME> should be the exact name of your firmware file.
   * <UPDATE_MAJOR_VER> should match the two first digits of the firmware version you're downgrading to, 04 in my case.
   * <UPDATE_MINOR_VER> should match the last 4 digits of the firmware version, plus the dot inbetween, 31.05 in my case.
   * <GMT> I entered the date the firmware file was created, you can find that in the properties of the firmware file as well. In my case it was 11 May 2017 02:44:26 GMT
   * <CDN_URL> you need to add the filename of your firmware after "http://snu.lge.com:80/". In my case it was "http://snu.lge.com:80/starfish-atsc-secured-k2l-20.deua.k2l-2030-04.31.20-prodkey_usb_V3_SECURED.epk"
  
6. Go to "C:\LgDTVUpDater" and double click on "start.bat"

7. Answer N to the first question and press Enter.

8. Look at the IP address at the start of the next question. Verify that this matches the local IP of your computer. If the address matches, answer Y and press Enter.

9. Repeat the previous step. If the IP matches, answer Y and press Enter.

10. Go back to your PC and press Enter in the command window with yellow text. If you get any Firewall warnings, allow them all through the firewall.

11. Go back to the TV and verify that it connects to the internet. All 4 rings should be green in the window you entered the computer IP.

12. If it connects, go to General -> About This TV, and click "Check for updates". Hopefully it finds the firmware you want to downgrade to. Click "Install".

13. If all is well, it should start downgrading!

I recommend resetting the TV to Initial Settings after downgrading, to be sure it works as intended.

# Credits
* Deepo from neogaf forums
* dkwong from AVSforum
* All the users in AVSforum c6 b6 owners thread
* Who ever made the LGDTVUpdater
