## dockerで手軽にrailsを試してみる時の手順

### 起動
docker run -it --name rails -p 3000:3000 -v /Users/miyajimatakeshi/work/:/work ruby:2.2.2 /bin/bash 

### 再起動
docker start rails

### 接続
docker exec -it rails bash

### 停止
docker stop rails
