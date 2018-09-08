# dns note
# 常見的正解檔 RR 相關資訊
[domain]    IN  [[RR type]  [RR data]]
主機名稱.   IN  A           IPv4 的 IP 位址
主機名稱.   IN  AAAA        IPv6 的 IP 位址
領域名稱.   IN  NS          管理這個領域名稱的伺服器主機名字.
領域名稱.   IN  SOA         管理這個領域名稱的七個重要參數(容後說明)
領域名稱.   IN  MX          順序數字  接收郵件的伺服器主機名字
主機別名.   IN  CNAME       實際代表這個主機別名的主機名字.

netstat -utlnp | grep named


[root@www ~]# vim /etc/named.conf						
// 在預設的情況下，這個檔案會去讀取 /etc/named.rfc1912.zones 這個領域定義檔						
// 所以請記得要修改成底下的樣式啊！						
options {						
	listen-on port 53  { any; };     //可不設定，代表全部接受					
	directory          "/var/named"; //資料庫預設放置的目錄所在					
	dump-file          "/var/named/data/cache_dump.db"; //一些統計資訊					
	statistics-file    "/var/named/data/named_stats.txt";					
	memstatistics-file "/var/named/data/named_mem_stats.txt";					
	allow-query        { any; };     //可不設定，代表全部接受					
	recursion yes;                   //將自己視為用戶端的一種查詢模式					
	forward only;                    //可暫時不設定					
	forwarders {                     //是重點！					
		168.95.1.1;              //先用中華電信的 DNS 當上層				
		139.175.10.20;           //再用 seednet 當上層				
	};					
	allow-transfer  { none; };   // 不許別人進行 zone 轉移			是否允許來自 slave DNS 對我的整個領域資料進行傳送？這個設定值與 master/slave DNS 伺服器之間的資料庫傳送有關。除非你有 slave DNS 伺服器，否則這裡不要開放喔！因此這裡我們先設定為 none。		
};  //最終記得要結尾符號！						
