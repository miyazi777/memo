
# macにhomebrewでmysqlをインストールした時のメモ
## インストール
brew install mysql

## 設定ファイルの場所
/etc/my.cnf

## 起動
mysql.server start



# mysqlslapのオプションについて

--concurrency	同時接続数
--number-of-queries	全体でのクエリ発光数
--engine	ストレージエンジン
--iterations	テストの実施回数
--auto-generate-sql-load-type	readならselectを行う性能テスト
--auto-generate-sql-write-number	各スレッドごとの行挿入回数(default=100)
-T または --debug-info	テスト終了時にメモリ・CPU使用率の統計を表示


# amazon linuxにmysql5.7をインストールするメモ

## インストール
amazon linuxはベースがcentos6ベースなので、何かのパッケージをインストールする際にはcentos6のパッケージを導入するとうまくいきやすい。
```
sudo yum -y install http://dev.mysql.com/get/mysql57-community-release-el6-7.noarch.rpm
sudo yum -y install mysql-community-server
```


## 起動
```
sudo service mysqld start
```

## 自動起動設定
```
sudo chkconfig mysqld on
```

## mysqlへログイン
以下のように/var/log/mysqld.log内にtemporary passwordとしてrootのパスワードが生成されているのでそれでログインする

```
sudo grep 'temporary' /var/log/mysqld.log
mysql -u root -p'&9i&7L:Qowml'
```

### rootの初期パスワードを変更
mysqlにログイン後、以下のパスワードで変更すること。パスワードにはある程度の強度が求められるので注意。
alter user root@localhost identified by 'Miyazix10x!';


ユーザーを追加
------------------------------------------

