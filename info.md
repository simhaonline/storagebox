sudo su -c 'apt update'
sudo su -c 'apt -y install autofs cifs-utils'

# mount-hetzner-storage
mkdir /backup

echo 'username=u345458
password=MkZ31h1RgxxNUErc' > /root/.cifs.txt

chmod 700 /root/.cifs.txt

sudo mount.cifs -o credentials=/root/.cifs.txt,iocharset=utf8,cache=none,rw,nounix,uid=0,gid=0,file_mode=0777,dir_mode=0777 //u345458.your-storagebox.de/backup/plesk /backup

echo '/cifs     /etc/auto.cifs    --timeout=60' >> /etc/auto.master

echo 'cifs credentials=/root/.cifs.txt,iocharset=utf8,cache=none,rw,nounix,uid=0,gid=0,file_mode=0777,dir_mode=0777 //u345458.your-storagebox.de/backup/plesk /backup' > /etc/auto.cifs

systemctl enable autofs && systemctl start autofs && systemctl status autofs

echo '//u345458.your-storagebox.de/backup/plesk /backup cifs iocharset=utf8,cache=none,rw,nounix,credentials=/root/.cifs.txt,uid=0,gid=0,file_mode=0777,dir_mode=0777 0 0' >> /etc/fstab

sudo mount -av

sudo df -hT
