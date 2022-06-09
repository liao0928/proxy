# proxy
b1042029 b1042048
- [proxy](https://github.com/liao0928/proxy/edit/main/README.md#proxy)
- [下載squid](https://github.com/liao0928/proxy/blob/main/README.md#%E4%B8%8B%E8%BC%89squid)
- [進入 /etc/squid/squid.conf](https://github.com/liao0928/proxy/blob/main/README.md#%E9%80%B2%E5%85%A5-etcsquidsquidconf)
- [進入blocksite](https://github.com/liao0928/proxy/blob/main/README.md#%E9%80%B2%E5%85%A5blocksite)
- [重置squid](https://github.com/liao0928/proxy/blob/main/README.md#%E9%87%8D%E7%BD%AEsquid)
- [多人proxy](https://github.com/liao0928/proxy/blob/main/README.md#%E5%A4%9A%E4%BA%BAproxy)
- [設置臨時代理](https://github.com/liao0928/proxy/blob/main/README.md#%E8%A8%AD%E7%BD%AE%E8%87%A8%E6%99%82%E4%BB%A3%E7%90%86)
- [設置永久代理](https://github.com/liao0928/proxy/blob/main/README.md#%E8%A8%AD%E7%BD%AE%E6%B0%B8%E4%B9%85%E4%BB%A3%E7%90%86)
- [永久設置代理訪問](https://github.com/liao0928/proxy/blob/main/README.md#%E6%B0%B8%E4%B9%85%E8%A8%AD%E7%BD%AE%E4%BB%A3%E7%90%86%E8%A8%AA%E5%95%8F)
- [APT設置代理](https://github.com/liao0928/proxy/blob/main/README.md#apt%E8%A8%AD%E7%BD%AE%E4%BB%A3%E7%90%86)
###### sudo apt-get update
先用ifconfig找出自己的ip，若不能用ifconfig請先下載apt install net-tools -y
###### sudo apt install net-tools -y
###### ifconfig
## 下載squid
###### sudo apt-get install squid
# ![image](https://i.imgur.com/O43tTGk.jpg)
進入root:
###### sudo su
## 進入 /etc/squid/squid.conf
###### gedit /etc/squid/squid.conf
# ![image](https://i.imgur.com/8tt0V3n.jpg)
到1407行，輸入:
# ![image](https://i.imgur.com/xkIKYN2.jpg)
###### acl local src 你的ip
###### acl blocksite dstdomain "/etc/squid/blocksite"
###### htttp_access deny blocksite
###### http_access allow localnet
到1420行，將http_access deny all改成:
###### http_acess allow all
# ![image](https://i.imgur.com/J7f2BcG.jpg)
# ![image](https://i.imgur.com/wXtZe57.jpg)
按f6搜尋:[http_port]，找到http_port ----改成http_port 3128
# ![image](https://i.imgur.com/z7xBCiJ.jpg)
接著儲存
回終端機後，輸入:
###### gedit /etc/squid/blocksite
## 進入blocksite
# ![image](https://i.imgur.com/u7Mo2BB.jpg)
# ![image](https://i.imgur.com/UQu1q3r.jpg)
輸入www.youtube.com 後儲存
# ![image](https://i.imgur.com/sy180su.jpg)
## 重置squid
###### systemctl restart squid
到firefox->setting->手動proxy->輸入自己ip,3128->儲存
# ![image](https://i.imgur.com/HazmwqH.jpg)
開youtube，得到的結果會是youtube開啟不了
# ![image](https://i.imgur.com/CDMF6PQ.jpg)
為了驗證並非網路有問題，回終端機再輸入:
###### gedit /etc/squid/blocksite
輸入www.facebook.com後儲存
# ![image](https://i.imgur.com/LuCHVLe.jpg)
反回終端機後輸入:
###### systemctl restart squid
開facebook，應該會得到開啟不了的結果

# 多人proxy
首先開啟setting->Network->Network proxy->Manual
在http,https,ftp欄都輸入: 你的ip,3128
# ![image](https://i.imgur.com/utbgTVK.jpg)
之後退出(按叉)
## 設置臨時代理
回到終端機，接著要為單個用戶設置臨時代理，輸入:
###### export HTTP_PROXY=[username]:[password]@[你的ip]:3128
###### export HTTPS_PROXY=[username]:[password]@[你的ip]:3128
###### export FTP_PROXY=[username]:[password]@[你的ip]:3128
###### export NO_PROXY=localhost,127.0.0.1,::1
## 設置永久代理
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
## 永久設置代理訪問
接著要為所有使用者(除了ubuntu以外的主機，如window)永久設置代理訪問
在終端機輸入:
###### sudo nano /etc/environment
進入後輸入:
###### export http_proxy="username:password@你的ip:3128"
###### export https_proxy="username:password@你的ip:3128"
###### export ftp_proxy="username:password@你的ip:3128"
###### export no_proxy="localhost, 127.0.0.1,::1"
輸入後退出
## APT設置代理
接著要為APT設置代理，在某些系統上，apt命令執行程式需要單獨的代理配置，因為它不使用系統環境變數，要為 apt 定義代理設置。
###### sudo nano /etc/apt/apt.conf
###### Acquire::http::Proxy "http://[username]:[password]@[你的ip]:3128";
###### Acquire::https::Proxy "http://[username]:[password]@[你的ip]:3128";
儲存並退出
接著在window或自己的主機開啟youtube、facebook會得到開啟不了的結果，但其他網頁可以
