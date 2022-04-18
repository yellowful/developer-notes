# docker

1. 傳統：
   1. virtualization, virtualized machine, sandbox environment
   2. 例如：vmware, virtualbox
   3. 每一個app有自己的guest OS
   4. 甚至開機都要幾分鐘
2. 現代：
   1. containers
   2. 例如：docker, k8s
   3. 只有一個OS，上面可以開很多不同的環境設定，給不同的app使用。
   4. 啟動只要幾秒
   5. 想要scale up，想開幾個container都可以輕鬆開。
3. 結構：
   1. host：機器、主機、伺服器
   2. container：在主機之上建立的，這個container裡面的環境是完全和host的環境是隔絕。
   3. image：
      1. 在container裡面
      2. 是由docker做出來的檔案，讓docker可以根據這個image建立相關環境，並且可以把我們寫出來的應用程式和images綁在一起。
      3. [docker hub](https://hub.docker.com/)可以抓各種環境的image回來用，例如：node各種版本、postgre各種版本
      4. 具有可讀寫的檔案系統，讓我們可以像使用一般電腦檔案一樣的使用
   4. 這個section要做：
      1. 依這個結構，在這個機器上建一個container，這個container裡面有nodejs的api、postgre、redis，三個裝在一個container裡，而只要一道命令就可以把這個環境run起來了。
      2. 業界很常的作法是前端會透過loadbalancer可以接到很多個container的nodejs的api，然後nodejs把資料導向Redis和postgre。
      3. Redis處理cache和session。
4. 練習docker相關命令：
   1. [安裝docker](https://docs.docker.com/desktop/mac/install/)
   2. 建立`Dockerfile`，例如：

      ```dockerfile
      FROM node:16.14.2
      WORKDIR /usr/src/docker-exercise
      COPY ./ ./
      RUN npm install
      CMD ["/bin/bash"]
      ```

   3. `WORKDIR`可以任意建立和指定，讓docker可以把檔案放在這個目錄下。
   4. 在docker hub裡找到需要的版本的名稱，寫進這個檔案裡：`FROM node:carbon`。這又稱為parent image。
   5. `CMD ["/bin/bash"]`：代表要在image裡要下什麼指令。
   6. `docker build -t container_name .`
      1. 用來建立container：`-t`代表給它一個tag。
      2. [其它用法](https://docs.docker.com/engine/reference/builder/#usage)
      3. `docker image ls`可以查詢所有build出來的images。
      4. `docker image rm image_hash`：可以刪除image。
   7. `docker run -it container_name`：
      1. 啟動container。
      2. 沒有加`-it`的話，沒辦法進入`tty`環境，也就是沒辦法進入container裡面的命令模式下指令。
      3. 在裡面下`exit`，container會消失。
      4. `docker run -it -d container_name`：會在背景執行
   8. container裡面：
      1. `docker run -it container_name`之後會發現命令提式變了，docker把我們帶向container裡面的環境了。
      2. 命令提式出現的hash是讓我們知道現在是在哪一個container裡。
      3. 下命令檢查node版本：`node -v`
      4. 會發現node版本不是我們要的`carbon`版，把第一行改成`FROM node:8.11.1`就正確了，這告訴我們，在docker裡一定要檢查環境是不是我們希望的。
      5. `cat /etc/*release`發現裡面linux的版本是linux 8，Debian。
   9. `docker ps`：可以查看所有在run的container
   10. [不小心砍掉bash檔怎麼辦](https://unix.stackexchange.com/questions/398543/what-are-the-contents-of-bin-bash-and-what-do-i-do-if-i-accidentally-overwrote)
   11. 只要把這個Dockerfile分享給任何一個人，他都可以用這個檔案build出一個一樣的環境，並且run。
   12. `docker exec`：
       1. 可以進入已經在run的container。
       2. 例如`docker exec -it hash_name bash`
   13. `docker stop hash_name`：停止container。
5. 把docker用在專案上：
   1. `WORKDIR /usr/src/smart-brain-api`：指定在container裡的哪個資料夾下執行。
   2. `COPY ./ ./`：把專案的所有檔案拷貝到container裡面相對應的位置，第一個目錄是`Dockerfile`所在的目錄，第二個目錄是container裡面的目錄。
   3. `RUN npm install`：代表我們在container裡面要執行`npm install`。
   4. `RUN`和`CMD`有什麼不同？
      1. `RUN`代表的是build step。
      2. 一個Docker image可以有很多的`RUN`，一個接著一個，建造出我們要的image。
      3. `CMD`是docker在launch的時候，才執行的指令，通常只會有一個，而且在Dockerfile的最下面。
   5. 在docker裡面執行`npm run start`，會發現我們的瀏覽器仍然無法連到localhost的port 3000，這是因為docker是在一個隔絕的環境裡面，不是在我們這個mac的環境裡，container並不知道我們這台host在哪，所以我們需要告訴container，要去哪裡曝露port，有三種方法：
      1. 最簡單是下命令時加上port的參數：
         1. `docker run -it -p 3000:3000 container_name`
         2. 會直接進入docker的環境中，在裡面就不是下docker的指令，而是下node的指令。
         3. 如果發現瀏覽器抓不到網頁，注意可能是docker環境裡面，沒去下`npm start`的指令，下了就可以run了。
      2. port binding
      3. port forwarding
   6. 要注意一點，我們在我們專案上修改code時，會發現沒有更新，那是因為我們只有在我們的環境裡修改，並沒有修改docker上的，只需要重build和run container就可以了。
6. Docker Compose：
   1. compose的每一個環境像是套在同一個container上，但實際上是在不同的container上，或許可以用一個docker world來理解。
   2. mac或是windows版，安裝docker之後就內含docker compose了，但是linux就要另外安裝。
   3. 建立一個`docker-compose.yml`檔，yml唸作yamal。
   4. 組織各種container的英文用語叫orchestrate，精心策劃的意思。
   5. 基本指令：
      1. `docker-compose build`：[建立所有container](https://docs.docker.com/compose/reference/build/)，每次設定有更改都要執行一次。
      2. `docker-compose run container_service_name`：啟動`docker-compose.yml`裡面的[某個service](https://docs.docker.com/compose/reference/run/)，比較少用。
      3. `docker-compose down`：把所有container都關掉，避免衝突。
      4. `docker-compose up --build`：[啟動所有services](https://docs.docker.com/compose/reference/up/)，比run常用。
      5. 範例：

        ```yml
        # 新版本不需要版本號了
        # version: '3.6'
        services:
          # service的名稱
          docker-exercise-api:
            # container的名稱
            container_name: cool_compose
            # 代表把在docker-compose.yml所在位置的Dockerfile檔放到container裡面
            build: ./
            # 要run的指令
            command: npm start
            # 要copy Dockerfile裡面的WORKDIR到這裡，讓他們一致，讓docker-compose找得到package.json
            working_dir: /usr/src/docker-exercise
            # 前面是container的port，後面是host的port
            ports:
              - "3000:3000"
            # 冒號前面是host的檔案位置，冒號後面是container在host的image位置，這樣host的檔案有變動的話，docker也會自動map給container
            volumes:
              - ./:/usr/src/docker-exercise
        ```

      6. 其它：
         1. `image: node:8.11.1`：但是現在是用其他的Dockerfile來build，所以這裡不用。
         2. [volumes的不同用法](https://stackoverflow.com/questions/34809646/what-is-the-purpose-of-volume-in-dockerfile)
         3. [深入研究volumes](https://www.linux.com/topic/cloud/docker-volumes-and-networks-compose/)
         4. 