# notebook :kissing:
* [Markdown tip](https://github.com/guodongxiaren/README)
* .vimrc and .screenrc: ```git clone https://github.com/wayne43290/vim-screen.git```
* Switch LTE Dongle to modem mode in Linux: ```sudo usb_modeswitch -J -v $(lsusb)_prefix -p $(lsusb)_postfix```
* [Cubieboard setting wifi](http://bigbata.com/blog/2014/05/17/cubieboard-begining-on-lubuntu-setup-wifi/)

## Git :blush:
Git: client error, server certificate verification failed:
https://fabianlee.org/2019/01/28/git-client-error-server-certificate-verification-failed/

## Bash :blush:
```Bash
make -p/-qp/-p -f /dev/null #To print out the value of parameters in Makefile:
```
To print the data base (rules and variable values) that results from reading the makefiles; then execute as usual or as otherwise specified. This also prints the version information given by the ‘-v’ switch.  
To print the data base without trying to remake any files, use `make -qp`.  
To print the data base of predefined rules and variables, use `make -p -f /dev/null`. The data base output contains file name and line number information for recipe and variable definitions, so it can be a useful debugging tool in complex environments.											

```Bash
du -sh * | sort -h                  #count size for the folders, and sort by size
scp $localFile $remoteUserName@remoteIP:$dir/$newFileName   #copy local file to remote computer
scp $remoteUserName@remoteIP:$dir/$fileName $dir/$newFileName   #copy remote file to local computer
grep -inr target_string /directory  #對檔案做內文搜尋，i忽略大小寫，w完全符合，n顯示該字串於檔案中的位置(行數)，r遞迴找
tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/log.txt 2>&1 &   #執行tar來壓縮某資料夾，放進背景執行並且把stderr與stdout都放到log.txt檔理頭
find /home/pat -iname "*.conf" | less #BJ4
ls -al /etc | tee output | tail	    #tee將指令結果輸出至螢幕也寫到output，再由tail/less部分顯示
sudo lshw -html > arthurtoday.html	#看硬體規格
```

## Remove pagkages :smile:
```Bash
sudo apt-get remove texlive-full    #在 Ubuntu 下移除某個軟體套件
sudo apt-get autoremove	            #移除一併自動安裝相依套件（dependencies）
sudo apt-get purge texlive-full	    #移除設定檔
sudo apt-get remove --auto-remove --purge #cleanest
```
```Bash
#對於先前用 autoremove 或 remove 或其它方式移除，但還沒經過 purge 徹底移除的套件，使用 dpkg 指令可以列出清單：
dpkg -l | grep ^rc	#^rc 代表行首以 rc 標示開頭，這是只有 remove 沒有 purge 的意思
#要批次移除這些被標為 rc 的套件，可以配合 grep + awk 指令。
dpkg -l | grep ^rc | awk '{ print $2 }'
#指令組合後即可批次徹底移除這些殘留套件。
sudo apt-get purge `dpkg -l | grep ^rc | awk '{ print $2 }'`
#如此，就可以讓系統稍微乾淨一點！
```

## Network :satisfied:
```Bash
ebtables -A FORWARD -i eth0 -o eth1 -p ip -j DROP   # to disable two ports communicate directly at bridge level.
ebtables -F FORWARD		                              # flush the ebtables rules
#/etc/hostname, /etc/hosts, /etc/resolv.conf        #系統維護DNS用的設定檔
```
```Bash
#persistently saving the iptables rules:
sudo iptables-save > /etc/iptables.rules
#add "pre-up iptables-restore < /etc/iptables.rules"
#    "post-down iptables-save > /etc/iptables.rules" into /etc/network/interfaces
```

Restore Ubuntu 14's network manager:
```Bash
sudo nm-connection-editor
sudo /etc/init.d/networking restart
vim /etc/NetworkManager/NetworkManager.conf
managed=false --> managed=true
sudo killall NetworkManager
```

Pi3: enable wifi and eth
```Bash
# To connect to certain wifi on boot
$ sudo vim /etc/wpa_supplicant/wpa_supplicant.conf

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
        ssid="howhow"
        psk="rabbithow"
}

[option 1]
sudo systemctl status dhcpcd.service    # To see the dhcp_client daemon status
# Do not type anything in /etc/network/interface
# To assign a static IP to a interface:
$ sudo vim /etc/dhcpcd.conf   #Follows the example there. E.g., 

interface eth1
static ip_address=192.168.4.2/24
profile static_eth1
static ip_address=192.168.4.2/24
interface eth1
fallback static_eth1

[option 2]
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```

MySQL: Sync for Master & Slave
```Bash
# Master/Slave settings, plz follows: https://blog.toright.com/posts/5062
# MySQL Commands: http://note.drx.tw/2012/12/mysql-syntax.html

# login mysql first
change master to master_host='$master_node_ip', master_user='$user_name', master_password='$pw_for_that_user', maser_log_file='mysql-bin.XX', master_log_pos=XX;
```
