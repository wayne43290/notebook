# notebook
A secret notebook
* [Readme.md markdown tip](https://github.com/guodongxiaren/README)
* .vimrc and .screenrc
```Bash
git clone https://github.com/wayne43290/vim-screen.git
```
* Switch LTE Dongle to modem mode in Linux
```Bash
sudo usb_modeswitch -J -v $(lsusb)_prefix -p $(lsusb)_postfix
```
* [Cubieboard setting wifi](http://bigbata.com/blog/2014/05/17/cubieboard-begining-on-lubuntu-setup-wifi/)

## Bash :blush:
### To print out the value of parameters in Makefile:
```Bash
make -p/-qp/-p -f /dev/null
```
To print the data base (rules and variable values) that results from reading the makefiles; then execute as usual or as otherwise specified. This also prints the version information given by the ‘-v’ switch.  
To print the data base without trying to remake any files, use `make -qp`.  
To print the data base of predefined rules and variables, use `make -p -f /dev/null`. The data base output contains file name and line number information for recipe and variable definitions, so it can be a useful debugging tool in complex environments.											

`du -sh * | sort -h`	# count size for the folders, and sort by size												
`scp $localFile $remoteUserName@remoteIP:$dir/$newFileName` # copy local file to remote computer
`scp $remoteUserName@remoteIP:$dir/$fileName $dir/$newFileName` # copy remote file to local computer
```Bash
grep -inr target_string /directory			# 對檔案做內文搜尋，i忽略大小寫，w完全符合，n顯示該字串於檔案中的位置(行數)，r遞迴找
tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/log.txt 2>&1 &	# 執行tar來壓縮某資料夾，放進背景執行並且把stderr與stdout都放到log.txt檔理頭
find /home/pat -iname "*.conf" | less
```
ls -al /etc | tee output | tail	#tee將指令結果輸出至螢幕也寫到output，再由tail/less部分顯示
To make certain service starts on boot: systemctl enable ssh.socket
移除篇:
sudo apt-get remove texlive-full	#在 Ubuntu 下移除某個軟體套件
sudo apt-get autoremove	#移除一併自動安裝相依套件（dependencies）
sudo apt-get purge texlive-full	#移除設定檔
sudo apt-get remove --auto-remove --purge
sudo apt-get autoremove --purge
對於先前用 autoremove 或 remove 或其它方式移除，但還沒經過 purge 徹底移除的套件，使用 dpkg 指令可以列出清單：
dpkg -l | grep ^rc	#^rc 代表行首以 rc 標示開頭，這是只有 remove 沒有 purge 的意思												
要批次移除這些被標為 rc 的套件，可以配合 grep + awk 指令。													
dpkg -l | grep ^rc | awk '{ print $2 }'													
指令組合後即可批次徹底移除這些殘留套件。													
sudo apt-get purge `dpkg -l | grep ^rc | awk '{ print $2 }'`													
如此，就可以讓系統稍微乾淨一點！

網路篇:													
ebtables -A FORWARD -i eth0 -o eth1 -p ip -j DROP			# to disable two ports communicate directly at bridge level.										
ebtables -F FORWARD		# flush modifications											
netstat -tulpn | grep 6633 #看port 6633有沒有人在用													
etc/hostname, etc/resolv.conf, etc/hosts 系統設定檔維護DNS用													
Persistent iptables: iptables-save > /etc/iptables.rules ; vim /etc/network/interface: add "pre-up iptables-restore < /etc/iptables.rules post-down iptables-save > /etc/iptables.rules"													

工具篇:
sudo lshw -html > arthurtoday.html	#看硬體規格												
