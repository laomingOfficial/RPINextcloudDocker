# 樹莓派OMV Docker架設NextCloud網盤

先説明，NextCloud上傳速度是硬傷，目前老明還沒解決方案。感人~  
下載滿速，可挂載samba。  
 
```
# 創建nextcloud
sudo docker-compose up -d

# 添加新URL
sudo nano /srv/dev-disk-by-label-120GB/dockerTest/volumes/rpinextclouddocker_nextcloud/_data/config/config.php

# trusted_domains的array裏添加
1 => 'server1.example.com',

# 在installed之後添加
'filesystem_check_changes' => 1,


# 安裝SMB
sudo docker exec -it nextcloud-app /bin/bash

# 輸入以下指令
apt update
apt install -y libsmbclient-dev
pecl install smbclient
echo "extension=smbclient.so" > /usr/local/etc/php/conf.d/docker-php-ext-smbclient.ini
exit

# 重啓
sudo docker restart nextcloud-app

# 刪nextcloud
sudo docker-compose down
sudo docker volume rm rpinextclouddocker_db rpinextclouddocker_nextcloud
```