# ssh連線

1. raspbian server安裝ssh：
   1. <https://www.makeuseof.com/tag/beginners-guide-setting-ssh-linux-testing-setup/>
      1. 安裝：`sudo apt install openssh-server`
      2. 狀態：`sudo systemctl status ssh`
      3. 打開防火牆：`sudo ufw allow ssh`
      4. 有時候需要：sudo systemctl enable ssh
   2. ngrok：<https://medium.com/coding-with-fun-favor/%E9%97%9C%E6%96%BC-ngrok-%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B-da12e19e4340>
      1. 用來讓local host獲得domain。
      2. 方法是註冊帳號，取得一個token，在local安裝一個ngrok的軟體，並且設定了token，ngrok就會動態把local publish一個網址出去。
      3. `sudo ./ngrok tcp 22`會得到`tcp://xxx.xxx.xxx.xxx:port -> localhost:22`。
      4. 回到mac，就可以用`sudo ssh pi@xxx.xxx.xxx.xxx -p port`
      5. <http://127.0.0.1:4040/inspect/http>可以看狀態。
2. 在server上設定ssh：
   1. 關於ssh：
      1. telnet只適合區網內使用，不安全。
      2. ssh是1995年才有的，適合在internet使用，他的package大小沒加密，內容加密，所以你可以監控連線資料，卻拿不到內容。
      3. 常見透過tcp連線，卻不一定要透過tcp連線，很多其它usb，rs232也都可以用ssh連線。
      4. 可以建立永遠的連線，可以穿越防火牆，可以同時建立很多平行雙向的連線，不衝突。也可以中斷一下，又連上之前已建立的連線，有各種應用。
   2. 進去或是建立`.ssh`的資料夾
   3. nano編輯authorized_keys
   4. 把public key貼上
   5. 在client裡面要設`ssh-add 私鑰檔案`，client端向server端連的時候，才會用這個私鑰驗證server端給的訊息。
3. [digitalocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04?mkt_tok=MTEzLURUTi0yNjYAAAF-bB6jd1svjcIUI6l82Mcr7BVqxIv6xY-lLdS11Fzon687ZhBhOM10mHuvbmtoLCwK0NfchRtN6T0dqy8wpd1bh56znpUqd1qk09SuKjJw8Q)：
   1. ssh key用github的就可以了
   2. 用key登入就不能用password登入
   3. 如果要某user可以用password登入就要編輯`/etc/ssh/sshd_config`：

      ```shell
         PasswordAuthentication no
         Match User richard,dq987921907
         PasswordAuthentication yes
         Match all
      ```

   4. 改完後`service ssh restart`
   5. 如果要另外用一個和給github不同的key專門給digital ocean的話，就和github步驟類似
      1. 先`ssh-keygen -t ed25519 -C "your_email@example.com"`，這裡-C是comment的意思，代表的是後面那個email。
      2. enter後，會要你取檔名，可以取`id_ed25519_digitalocean`
      3. `pbcopy < ~/.ssh/id_ed25519_digitalocean`
      4. 到remote裡面的`./ssh`裡，去找到authorized_keys，找不到就創一個，把public key貼進去。
      5. 登入不同host可以用這個指令切換不同的key：`ssh-add ~/.ssh/privatekey的名稱`
      6. `ssh-add -l`：列出所有做用中的key
      7. `ssh-add -D`：刪掉所有做用中的key，你再加入你要登入的key
4. tunnel有兩種：
   1. 遠端控制和跳版，例如：`ssh -tt pi@10.2.1.12` 再 `ssh -tt pi@10.2.2.10`，不斷的跳板到任何電腦。
   2. 另一種是用來port對應，以穿越防火牆。
      1. 例如讓遠端的電腦對應到本地端的某個port，這樣就可以在本地端看到遠端網站的頁面。
      2. 例如讓本地端的某個port對應遠端的某個port，這樣就可以把本地的網站publish到internet了。
5. [rsync](https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)：第12堂課
   1. 並不是像icloud一樣自動同步兩個資料夾。
   2. 是下不同的指令，讓兩個資料夾，以不同的方式來拷貝或同步。

## vpn

[globalprotect用在big sur](https://docs.paloaltonetworks.com/globalprotect/5-1/globalprotect-app-user-guide/globalprotect-app-for-mac/download-and-install-the-globalprotect-app-for-mac.html)

1. 你需要先有GlobalProtect portal(公司那一端，管理員那一端)給你的IP或是FQDN，才能下載和安裝GlobalProtect App。
2. Catalina以後的版本，或是GlobalProtect app5.1.4以後的版本，你第一次安裝GlobalProtect App的時候，會有提示，需要你啟動mac裡的system extenstions，這是GlobalProtect某些feature需要的。
3. 過程會像網頁裡的截圖一樣，先去登入，去下載頁安裝。
4. 重點：
   1. 用瀏覽器去要vpn的host的網址，會因憑證被擋而不能連，強制同意憑證的安全性就可以連了。
   2. 安裝時要勾選安裝GlobalProtect System Extensions。
   3. 跳出一個系統警告時，要選擇去安全性設定裡打開「同意globalprotect」。

[VPN Protocol](https://go-vpn.com/vpn-protocol/)

VPN有以下幾種不同的協定，要弄清楚是用哪一種才能連接：

1. OpenVPN：
   1. 每間VPN都支援。
   2. 受歡迎。
   3. 相對較新。
   4. 各種加密算法，安全。
   5. 較快。
   6. 設定較麻煩，要產生憑證。
2. Onion Over VPN：
   1. 絕對安全。
   2. 匿名，隱藏真實IP。
   3. 有提供的業者：NordVPN。
3. IKEv2：
   1. 最快。
   2. MS和Cisco共同開發。
4. PPTP：
   1. 點對點隧道協議(Point to Point Tunneling Protocol)。
   2. 快但不安全，不要用。
   3. 一組帳密就可以用了。
5. L2TP / IPSec：
   1. L2TP (Layer Two Tunneling Protocol, Level 2隧道協議)
   2. IPSec (Internet Protocol Security，網際網路安全協定)
   3. MS和Cisco共同開發。
   4. 比PPTP慢，不安全。
   5. L2TP要搭配IPSec比較有保障。
   6. 不要用。
   7. 設定要多一道共用的金鑰。
6. TLS/SSL：
   1. TLS/SSL (Transport Layer Security/Secure Sockets Layer)
   2. 所有瀏覽器都支援。

[設定VPN的方法](https://iqmore.tw/set-up-vpn-server-on-router)：

globalprotect：
1. 
2. 會發現不能