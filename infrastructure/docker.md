インストール
=====================================

docker toolbox
-------------------------------------

macやwindowsでdockerを動作させる為の一式が入っているツール


## インストール方法
・公式サイトからマシンごとのインストーラをダウンロードし、あとは画面の指示どおり。

### 試していないが、こちらでもいけるかも
brew cask install dockertoolbox


## インストールされるもの

* docker            dockerのコマンドラインツール
* docker-machine    dockerホストの作成・削除・起動・停止
* docker-compose    複数のコンテナを管理するツール
* virtual-box       仮装マシンツール



## インストール方法(この方法は非推奨になっている）
* 公式サイトでインストーラー経由でインストール
https://github.com/boot2docker/osx-installer/releases
* homebrew-caskがインストールされている時は以下のコマンドでインストール可能
	brew cask install boot2docker






docker machine
-------------------------------------

dockerホストの管理ツール
dockerホストの作成・削除・起動・停止を行うことができる

## docker machine command

### ホストの一覧を確認する
docker-machine ls

### ホストの起動
docker-machine start <machine-name>

### ホストの停止
docker-machine stop <machine-name>

### ホストを作成
docker-machine creawte --driver <作成先？> <machine-name>

#### 例
docker-machine creawte --driver virtualbox dev


### ホストの削除
docker-machine rm <machine-name>

### ホストの設定を表示
docker-machine env <machine-name>

#### このコマンドを使ってdockerコマンド用の環境変数を設定することができる
eval $(docker-machine env <machine-name>)
env | grep DOCKER

#### 環境変数を削除する歳には以下のコマンド
eval $(docker-mahcine env --unset)

### ホストの瀬亭を表示（dockerコマンドで使える形式)
docker-machine config <machine-name>
env | grep DOCKER

#### このコマンドを利用してdockerコマンドを実行することが可能
docker $(docker-machine config dev) ps

### ホストのipを取得
docker-machine ip <machine-name>

### ホストにssh接続
docker-machine ssh <machine-name>


docker
-------------------------------------

## command

### イメージのリストを確認
docker images

### イメージの検索（docker hub内のイメージを検索)
docker seach <image-name>

### イメージを持ってくる
docker pull <image-name>:<tag-name>

<tag-name>は省略可能。省略した場合はlatestが自動で指定される

### イメージの詳細を確認
dokcer inspect <image-name>

### イメージの作成
docker build -t <image-name> <作成先>

#### example
docker duild -t docker-whale .

### イメージからコンテナを作成
docker create <image-name>

### イメージからコンテナを作成・起動する
docker run <image-name>
#### --nameでコンテナに名称を付与する
docker run --name <container_name> <image-name>
#### -d でバックグラウンド起動となる
dockerは-dつけない場合、ターミナルを終了するとコンテナも停止することに注意
docker run -d centos /bin/ping localhost
#### -it でforeグラウンド起動となる
docker run -it --name "test2" centos /bin/bash
#### -pでポートのマッピングを行う
docker run -d -p 8080:80 httpd
上のコマンドではホスト側の8080ポートをコンテナの80ポートをマッピング

### イメージ削除
docker rmi <image-name>
ただし、イメージを元に作成したコンテナが存在する場合はエラーとなる

### イメージ名の変更
docker tag <old-name>:<tag-name> <new-name>:<tag-name>h



### コンテナ一覧確認
docker ps -a

-a(--all) は停止中のものも含む全てのコンテナ

### コンテナの起動
docker start <container-name>

### コンテナの再起動
docker restart <container-name>

### コンテナに接続
docker attach <container-name>

#### コンテナを抜ける時に
ctrl + p -> ctrl + qとか指定することでコンテナを終了せずにホストに戻れる
* 4/4 最近のdockerはexitで抜けてもコンテナが停止しない？ -> docker exec -it などで入った場合は抜けても停止しないが、attachで入った場合は停止する模様

#### バックグランドモードで起動しているコンテナに接続する場合には以下のコマンドの方が実用的
docker exec -it <container_name> bash

docker exec -it --sig-proxy=false <container_name> bash
--sig-proxy=falseでもし、ctrl+cで抜けてもコンテナが止まらない

### コンテナからイメージを作成
docker commit <container-name> <image-name>

### コンテナの停止
docker stop <container-name>

### コンテナの強制停止
docker kill <container-name>

### コンテナの削除
docker rm <container-name>

### コンテナ名の変更
docker rename <old-name> <new-name>

### コンテナの稼働状態を確認
docker stats <container-name>



# dockerに関する情報を表示する
docker info

# dockerのバージョン確認
docker version




Dockerfileについて
-------------------------------------

## Dockerfileを元にイメージを作成
docker build -t <image-name>:<tag-name> <Dockerfile-path>

## Dockerfileで使用できる命令
FROM <image-name>   ベースイメージを指定
RUN <command>       イメージを作成する為のコマンドを実行(yumやapt-getkど)
CMD <command>       生成されたコンテナ内で実行するコマンドを指定(webサーバなどのサービスの起動など)
ENTRYPOINT <command>    docker runされた時に実行されるコマンドを指定
ONBUILD <command>   dockerイメージがビルドされた時に実行されるコマンドを指定
ENV <env-name> <value>  環境変数を設定する
WORKDIR <path>      作業ディレクトリの指定
USER <user-name>    ユーザーの指定
EXPOSE <port-no>    コンテナを公開するポートの指定
ADD <host-filepath> <image-filepath>    ファイル・ディレクトリの追加
COPY <host-filepath> <image-filepath>   ホストのファイルをイメージ内の指定ディレクトリへコピー

`# コメント

Dockerfileの作り方(page1)
-------------------------------------

## ベースイメージを元にコンテナ内部で作業
docker run -it --name work-con centos:6 /bin/bash
上のコマンドでコンテナを起動し、その上でyumコマンドなどでインストールしたいものを入れていく。
yumコマンドなどのインストールコマンドはどこかにメモしておく。
もし、インストールに失敗した場合はexitで抜けてもう１度、上記コマンドでコンテナを作り直して再作業。

### さらにバックグラウンドで起動し、ポートも指定した上でコンテナを作成することも可能
```
docker run -itd --name work-con -p 8080:80 centos:6 /bin/bash
docker run -itd --name test -p 3000:3000 centos:latest /bin/bash
```

## 途中の状態を保存、または作業が終わったら
ctrl+p -> ctrl+qでコンテナを停止しないようにホストに戻った後、
docker commit work-con work-image
イメージとしてコンテナの状態を残すことができる。

## Dockerfileを作成
下のようなDockerfileを作成する。
ちなみにこのファイルはnginxをインストールし、起動する
```
FROM centos:6

RUN set -x && \
    yum install -y epel-release && \
    yum install -y nginx

EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

## Dockerfileを元にイメージを作成
docker build -t test-image .

## 作成したイメージを元にコンテナ起動
docker run --name test-con -d -p 80:80 test-image

これでdocker-machine ip <machine-name>で表示されるipにブラウザからアクセスするとnginxの標準画面が表示される

docker-compose
-------------------------------------

## コマンド
docker-compose build <path>   docker-compose.yml内でbuildが指定されているものを全てビルドする
docker-compose up       docker-compose.ymlで指定されたコンテナ起動
docker-compose run
docker-compose <command>    DockerfileのCMDを上書きする
docker-compose environment  container内で環境変数を追加する
docker-compose volumes      hostのフォルダをcontainerにmountする
docker-compose exec <container_name> <command>

## docker-compose.yml
build: <host-Dockerfile-path>  Dockerファイルを元にイメージをビルド
container_name: <container-name>    コンテナ作成時の名前をここで指定
image: <image-name> コンテナ作成元のイメージを指定
extra-hosts: <dns record> コンテナ内のhostsは追加することができないので、この設定で追加する





dockerの実例
=====================================

## ホストとコンテナのフォルダ共有
以下のコマンドで可能
docker run -it -v /Users/miayjimatakeshi/study/docker/sharevol/:/tmp centos /bin/bash
docker run -it -v "$PWD":/tmp ruby:2.3.1 /bin/bash
docker run -it -v ~/.ssh:/root/.ssh ruby:2.3.1 /bin/bash

/Users/miyajimatakeshi〜はホスト側のフォルダ
/tmpはコンテナ側のフォルダ

### osx + virtualboxという環境だとものすごく共有フォルダがものすごく遅い対応
http://qiita.com/masuidrive/items/d71ee1881fffb6ad098f


## 証明書エラーが出た時の対応
dockerのコマンドを打つとタマに以下のようなエラーに遭遇することがある。
An error occurred trying to connect: Get https://192.168.99.100:2376/v1.22/images/json: x509: certificate is valid for 192.168.99.101, not 192.168.99.100

証明書が合致しないことが問題のようなので再作成する。
docker-machine regerate-certs dinghy(machine-name)

## 起動しているコンテナを全て停止
docker stop $(docker ps -aq)
docker stop `docker ps -a -q`

## 停止中のコンテナを一気に削除する
docker rm $(docker ps -aq)
docker rm `docker ps -a -q`

## noneとなっているイメージを一括削除
docker rmi $(docker images | awk '/^<none>/ { print $3 }')

## コンテナ -> ファイルコピー
ホスト側で以下のコマンドを別ターミナルで叩く
docker cp <container-name>:<container-file-path> <host-filepath>

### 例
docker cp work3:/etc/yum.repos.d/CentOS-Base.repo .

## データの永続化について
dockerはコンテナが削除されると保存されたデータが全て消える。
その為、データを格納する為だけのコンテナを作成し、このコンテナにデータを保存する。
なお、データ格納用のコンテナは特に起動しておく必要はない。

### 実験例
dbがmysqlのコンテナでdbdataがデータ格納用のコンテナ
```
docker-compose.yml

db:
    image: mysql:5.7.7
    volumes_from:
        - dbdata
    environment:
        MYSQL_ROOT_PASSWORD: mysql
    ports:
        - "3306:3306"
dbdata:
    image: busybox
    volumes:
        - /var/lib/mysql
```

### 起動方法
docker-compose up -d

### dbコンテナ内のmysqlへ接続
docker exec -it <container-name> bash

### dbコンテナへmysqlでの接続
mysql -h <ip> -uroot -p
<ip>はdocker-machine ipで確認


### dockerコンテナが起動しない時の原因究明方法
d logs <container-name>
でログを表示する

### docker上でbowerを使う時の注意点
ホストとフォルダ共有している場所ではbower installしても所有者が違う為にエラーになる。
フォルダ共有されていない場所でbower installした後、chown -R 501:dialout <folder-name>してから移動させるしかない。

### docker上で後からコンテナにとりあえず、環境変数を追加したい場合
基本、rootユーザーとなってしまうのでrootユーザーの.bashrcに環境変数を設定する
```
/root/.bashrc

export XXX=88
```

## 起動しているコンテナにログインする
個人的にこれまで知ってるやり方
```
docker exec -it codenberg_app_1 bash
```

実は以下のコマンドでもいける
```
docker-compose exec app bash
```


## docker起動したウィンドウが固まった時の対処方法
---------------------------------------

とりあえず、ctrl+p -> ctrl+q



気楽にruby環境を使うdockerコマンド
---------------------------------------

以下のいずれかのコマンドでコンテナ作成
```
docker run --name app-test -it -d -p 3000:3000 -v "$PWD":/app -w /app ruby:2.3.1 /bin/bash
docker run --name app-test -it -d -p 3000:3000 -v ~/.ssh:/root/.ssh -v "$PWD":/app ruby:2.3.1 /bin/bash
```

コンテナ内で作業する際には以下のコマンドで入り込む
```
docker exec -it app-test bash
```

元になったのは以下のコマンド
```
sudo docker run --name cdb-app -it -d -p 3000:3000 -v "$PWD":/usr/src/app -w /usr/src/app --link cdb-mysql-dev:db 52.193.209.49:5000/cdb-app:v2 /bin/bash
```

## alpine ベースのrubyコンテナを使う
docker run --name test -it -d -p 3000:3000 -v ~/.ssh:/root/.ssh -v "$PWD":/app ruby:2.3.1-alpine /bin/sh
docker run --name test -it -d -p 3000:3000 -v "$PWD":/app ruby:2.3.1-alpine /bin/sh

### gem開発用のコンテナ
docker run --name gem-test -itd -v "$PWD":/app ruby:2.3.1 /bin/bash
docker exec -it gem-test sh

## alpineベースのnginxコンテナを使う
docker run --name test -it -d -p 80:80 alpine-nginx /bin/sh


個人用のrails実験環境を作成
------------------------------------



Dockerの疑問点
-------------------------------------

## Dockerfileでredhat系のserviceコマンドが使えない？
Dockerfileに
CMD service nginx start
のように記述してdocker runしてもコンテナそのものが起動せずにすぐに停止する


