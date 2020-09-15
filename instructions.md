Create required folders for mounting the drives on
mkdir /home/pi/shares/nas_music

List devices and their UUID
sudo blkid 

Use the UUID you obtain to edit the fstab to automount the drives at reboot
UUID=E628473A2847094F /home/pi/shares/nas_essentials auto defaults,nofail,nobootwait 0 0
UUID=409A-F4AF /home/pi/shares/nas_music auto defaults,nofail,nobootwait 0 0
UUID=8CAB-602F /home/pi/shares/nas_entertainment auto defaults,nofail,nobootwait 0 0
UUID=8041-E1DE /home/pi/shares/nas_downloads auto defaults,nofail,nobootwait 0 0

Installing SAMBA
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install samba samba-common-bin

Configuring SAMBA to share your folders where you have mounted your drives
vi /etc/samba/smb.conf

[NAS_DOWNLOADS]
  comment = Downloads directory on Network 
  path = /home/pi/shares/nas_downloads
  create mask = 0660 
  directory mask = 0771
  valid users = @users 
  read only = no
[NAS_ENTERTAINMENT]
  comment = Movies and TVSeries on Network
  path = /home/pi/shares/nas_entertainment
  valid users = @users
  create mask = 0660
  directory mask = 0771 
  read only = no
[NAS_ESSENTIALS]
  comment = WD_BLACK_1TB on Network 
  path = /home/pi/shares/nas_essentials
  create mask = 0660
  valid users = @users
  directory mask = 0771 
  read only = no
[NAS_MUSIC]
  comment = Music on Network
  path = /home/pi/shares/nas_music 
  create mask = 0660
  valid users = @users
  directory mask = 0771 
  read only = no
  
  
Share your printer
https://pimylifeup.com/raspberry-pi-print-server/

Installing CUPS
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install cups

Add pi user to admin for CUPS
sudo usermod -a -G lpadmin pi

Allow accessing CUPS over the network
sudo cupsctl --remote-any
sudo systemctl restart cups

Accessing CUPS:
http://<rpi_IP>:631

Sharing printer using SAMBA

[printers]
   comment = All Printers
   browseable = yes
   path = /var/spool/samba
   printable = yes
   guest ok = yes
   read only = yes
   create mask = 0700

[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = yes

Installing Plex Media Server 
https://pimylifeup.com/raspberry-pi-plex-server/

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install apt-transport-https

curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
sudo apt-get update
sudo apt install plexmediaserver

Accessing Plex Media Server
<rpi_IP>:32400/web/
