# LANTurtleQuickCredsVariant
A variant of the 1.3 version available on the Hak5 GitHub

---

Usage:

Don't download the quickcreds module using the LAN Turtle text "GUI" menu system. Download this quickcreds file and place it in /etc/turtle/modules and when that is done, enter the LAN Turtle text "GUI" and configure the module and enable it. Then start using it.

---

Main changes made compared to the "official" Hak5 1.3 version that was available Feb 2021 on Hak5 GitHub:

- Using a more consistent way of indenting the script code so that it will be easy to read and follow

- Moving the function named "finished" above the function "start", since "start" calls for "finished" (it's bash basics to have modules called from within the same script before it's actually being called...)

- Added unzip to the dependencies installation procedure so that the most recent Responder master file from GitHub can be used (not archived tar.gz ones)

- Separated the loot so that it's not a subdir directly under /root/loot but instead using /root/loot/quickcreds/
  The Turtle may use several payloads in parallell (not at once, but installed) and it's nice to keep stuff apart and organized
  Also not just using plain numbers for subdirs, but prefixing them with "Creds", like "Creds1", "Creds2", etc.

- Added/increased the use of variables in the script code

- Being a bit more consistent when it comes to the if statements used (some are double square brackets, some not, some semicolon missing before "then"...), bash provides a more modern way than just using single brackets

- Changed the script so that it supports both Turtles with orange/amber and yellow LEDs (don't have a Turtle with a yellow LED, so not tested and verified)

- Since the latest versions of Responder are larger in size, the archive has also gotten bigger. Because of that the zip is downloaded to /tmp first which
  has more storage, then removing exe files not needed and after that moving Responder to the correct location under /etc/turtle
  We don't want to get stuck with Responder versions that are years old, so... fixing it...
  
- Full paths added to binaries - /usr/bin /usr/sbin /bin etc.

- Changed network interface from br-lan to eth0 (the NIC facing the USB port)

- The way that the script is set up on the Hak5 GitHub, it just simply can't work since the directory references to Responder is totally wrong, it unpacks to /etc/turtle/Responder/Responder-2.3.3.5 that makes the rest of the script unusable since it expects Responder to be in /etc/turtle/Responder

- Added sync to the "finish" function

- Adjusted the number handling when creating a numbered lootdir, if no existing dirs (i.e. wc -l = 0) then start with 1 instead of 0 otherwise there will be a gap between 0 and 2 dirs

- Changed ping from lanturtle.com to www.google.com

- The if statement containing __cat $RESPLOG | tail -n1) == \*"Creds"\*__ ... honestly... I don't know what value that adds really, the only "Creds" that is added to that file is AFTER that check in the same if statement... peculiar... so... removed...

- If running around in an office during an engagement collecting creds from several machines, then just run __cat /root/loot/quickcreds/responder.log | grep NTLM__ after the "tour" in order to get a quick picture of how many NTLM creds that were collected

Comments in general:
- The script doesn't capture creds for users on the same PC that isn't currently logged in (pretty logic though)
- The script doesn't work if the current user is logged in with a Microsoft account (i.e. an online account)
- You can "fool" the same Windows box only once, or... it's at least a lot more difficult and time consuming to try a second time
