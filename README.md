# 單人proxy
###### sudo apt-get update
###### sudo  
###### ifconfig
###### sudo apt-get install squid
![image](https://i.imgur.com/O43tTGk.jpg)
###### sudo su
###### gedit /etc/squid/squid.conf
![image](https://i.imgur.com/8tt0V3n.jpg)
到1407行，輸入:
![image](https://i.imgur.com/xkIKYN2.jpg)
###### acl local src 你的ip
###### acl blocksite dstdomain "/etc/squid/blocksite"![image]()
###### htttp_access deny blocksite
###### http_access allow localnet
到1420行，將http_access deny all改成:
###### http_acess allow all
![image](https://i.imgur.com/J7f2BcG.jpg)
![image](https://i.imgur.com/wXtZe57.jpg)
按f6搜尋:[http_port]，找到http_port ----改成http_port 3128
![image](https://i.imgur.com/z7xBCiJ.jpg)
接著儲存
回終端機後，輸入:
###### gedit /etc/squid/blocksite
![image](https://i.imgur.com/u7Mo2BB.jpg)
![image](https://i.imgur.com/UQu1q3r.jpg)
輸入:www.youtube.com後儲存
![image](https://i.imgur.com/sy180su.jpg)
重置squid:
###### systemctl restart squid
到firefox->setting->手動proxy->儲存->開youtube
![image](https://i.imgur.com/HazmwqH.jpg)
得到的結果會是youtube開啟不了
![image](https://i.imgur.com/CDMF6PQ.jpg)
為了驗證並非網路有問題，回終端機再輸入:
###### gedit /etc/squid/blocksite
輸入www.facebook.com後儲存
![image](https://i.imgur.com/LuCHVLe.jpg)
反回終端機後輸入:
###### systemctl restart squid
開facebook，應該會得到開啟不了的結果

# 多人proxy
首先開啟setting->Network->Network proxy->Manual
在http,https,ftp欄都輸入: 你的ip,3128
![image](https://i.imgur.com/utbgTVK.jpg)
之後退出(按叉)
回到終端機，接著要為單個用戶設置臨時代理，輸入:
###### export HTTP_PROXY=[username]:[password]@[你的ip]:3128
###### export HTTPS_PROXY=[username]:[password]@[你的ip]:3128
###### export FTP_PROXY=[username]:[password]@[你的ip]:3128
###### export NO_PROXY=localhost,127.0.0.1,::1
因通過終端視窗配置的代理設置會在重新啟動系統後重置。所以將單個用戶設置永久代理
輸入:
###### sudo nano ~/.bashrc
在bashrc檔底部添加以下：
###### export http_proxy="username:password@你的ip:3128"
###### export https_proxy="username:password@你的ip:3128"
###### export ftp_proxy="username:password@你的ip:3128"
###### export no_proxy="localhost, 127.0.0.1,::1"
接著儲存後退出，
然後執行以下命令將新設置應用於當前工作階段：
###### source ~/.bashrc
接著要為所有使用者(除了ubuntu以外的主機，如window)永久設置代理訪問
在終端機輸入:
###### sudo nano /etc/environment
進入後輸入:
###### export http_proxy="username:password@你的ip:3128"
###### export https_proxy="username:password@你的ip:3128"
###### export ftp_proxy="username:password@你的ip:3128"
###### export no_proxy="localhost, 127.0.0.1,::1"
輸入後退出
接著要為APT設置代理，在某些系統上，apt命令執行程式需要單獨的代理配置，因為它不使用系統環境變數，要為 apt 定義代理設置。
###### sudo nano /etc/apt/apt.conf
###### Acquire::http::Proxy "http://[username]:[password]@[你的ip]:3128";
###### Acquire::https::Proxy "http://[username]:[password]@[你的ip]:3128";
儲存並退出
接著在window或自己的主機開啟youtube、facebook會得到開啟不了的結果，但其他網頁可以
