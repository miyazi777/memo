# Vagrant Memo

## sshでログインするには？
sshコマンドで以下のようにログイン可能。

例：
<pre>
ssh -l vagrant 192.168.33.20 -p 22
</pre>


## sshの設定手順2(公開キーを登録する方法(LIGで紹介されていたやり方))
以下の手順で公開キーを登録すると.ssh/configを汚すことがないので便利

### Vagrantfileの内容
<pre>
config.vm.network :private_network, ip:"192.168.33.20"
</pre>

### コマンド
下記のコマンドで仮想マシンを起動し、.ssh/configへ設定追加
<pre>
vagrant up 
ssh-keygen -t rsa	※すでに公開キーが作られている時には不要
cat ~/.ssh/id_rsa.pub | ssh vagrant@192.168.33.20 "cat >> .ssh/authorized_keys"
ssh vagrant@192.168.33.20
</pre>

### hosts
<pre>
[servers]
192.168.33.20
</pre>

### playbook
下はmysqlをインストールして、自動起動する設定をしているplaybook
<pre>
- hosts: servers
  user: vagrant
  sudo: yes
  gather_facts: no
  tasks:
    - name: add epel package
      yum: name=http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm state=present
    - name: add remi package
      yum: name=http://rpms.famillecollet.com/enterprise/remi-release-6.rpm state=present
    - name: mysql-server installed
      yum: name={{ item }} state=installed enablerepo=remi
      with_items:
         - mysql-server
         - mysql-utilities
    - name: git installed
      yum: name=git state=installed
    - name: vim installed
      yum: name=vim state=installed
    - name: mysql service running
      service: name=mysqld state=running enabled=yes
</pre>

### playbook実行
<pre>
ansible-playbook playbook.yml
</pre>



## sshの設定手順

### Vagrantfileの内容
<pre>
  config.vm.network :private_network, ip:"192.168.33.10"
</pre>
  


### コマンド
下記のコマンドで仮想マシンを起動し、.ssh/configへ設定追加
<pre>
  vagrant up 
  vagrant ssh-config --host <hostname> >> ~/.ssh/config
</pre>

.ssh/configの設定でhostnameとportを以下のように変更

<pre>
Host <hostname>
  HostName 127.0.0.1 -> 192.168.33.10
  Post 2222 -> 22
</pre>

ssh <hostname>でログインできるハズ







## チュートリアル

マシン作成
vagrant init <box名>

マシン起動
vagrant up

ssh設定を追加
vagrant ssh-config --host ls1 >> ~/.ssh/config



## box名の変更方法

boxのファイルが~/.vagrant.d/boxes/以下に格納されているので、それをrenameするだけでOK


## Vagrantファイル

### ゲストOSのipアドレスを指定する

Vagrantfileに以下の記述を追加

<pre>
 # ホスト名指定
 config.vm.hostname = "sandbox"
 # ipアドレス指定
 config.vm.network :private_network, ip:"192.168.33.10"
</pre>





## 用語

|用語|内容|
|---|---|
|プロバイダ | 仮想環境のこと|
|プロビジョニング | 設定ツールのこと|
|BOXファイル	| 仮想環境を作成する際のベースとなるイメージファイル|
|Vagrantfile | vagrantを使って構築する仮想マシンのスペックや設定ツールなどを指定するファイル|




## インストール 

### 【前提】
virtualBoxがインストールされていること

### 公式サイトから各OSごとの最新版をダウンロードし、パッケージをダブルクリック
http://www.vagrantup.com/downloads.html

### バージョン確認
下記のコマンドでバージョンが表示されれば、インストール成功
vagrant -v 



## virtualBox上に仮想環境を作成する 

VirtualBox上にCentOS6.4を構築する

### (1)Boxファイル用のディレクトリを作成
以降はこのディレクトリ内で作業する


### (2)Boxファイルの追加
vagrant box add <box名> <urlまたはファイル名>

urlは下記のサイトで公開されているboxファイルのurlを指定
http://www.vagrantbox.es/

例:centos 6.4のboxイメージで仮想マシンイメージを作成
vagrant box add centos64 https://github.com/2creatives/vagrant-centos/releases/download/v0.1.0/centos64-x86_64-20131030.box



### (3)Vagrantfileの作成
下記のコマンドでVagrantファイルのひな形を作成する
vagrant init contos64


### (4)仮想マシンを起動する
vagrant up

### (5)起動後、以下のアカウントでログイン可能
id : vagrant
pass : vagrant

### (6)VMが起動している状態の場合、以下のコマンドでログイン可能
vagrant ssh

※ipを指定している場合はホストマシンから以下のようにしてログイン可能
ssh ユーザー名@ホスト名（ipアドレス)

例：
ssh vagrant@192.168.33.10




## プラグイン

|sahara|vagrantの変更管理ツール|
|vagrant-omnibus|chefがインストールされているか確認して、なければインストールしてくれる|




## vagrantコマンド


|コマンド|動作|
|---|---|
|vagrant -v|バージョン表示|
|vagrant -h	|ヘルプ表示|
|vagrant box add <box名> <url>	|boxファイルの追加※boxファイルのイメージはここで公開されている http://www.vagrantbox.es/|
|vagrant box list				|boxファイルのリスト表示|
|vagrant box remove <box名>		|boxファイルの削除|
|vagrant init <box名> |指定されたbox名のイメージをベースとしたVagrantファイルを作成する|
|vagrant up						|仮想マシンの作成・起動(初回起動時のみプロビジョニングが行われる)|
|vagrant provision				|仮想マシンの設定(先に仮想マシンを起動しておく必要がある)|
|vagrant ssh					|仮想マシンへログイン|
|vagrant status					|仮想マシンの状態を確認|
|vagrant halt					|仮想マシンを停止|
|vagrant reload					|仮想マシンの再起動|
|vagrant destory				|仮想マシンの削除|
|vagrant destory -f 			|強制的にマシンの削除|
|vagrant package			  |	現在の仮想マシンをboxファイルに出力する
|vagrant ssh-config --host <hostname> >> ~/.ssh/config              |ssh接続する為のconfigを出力してくれる|
|vagrant plugin install <プラグイン名>	|プラグインのインストール|
|vagrant plugin list					|プラグインのリストを表示|




## saharaプラグラインのコマンド 

### 概要
vagrantの状態管理を行うことができるようになる。例えば、変更を加えたが、失敗した場合などに元の状態へロールバックできたりする。

### コマンド
|コマンド|動作|
|---|---|
|vagrant sandbox on		|sandboxモードをon|
|vagrant sandbox off	|sandboxモードをoff|
|vagrant sandbox status	|sandboxの状態を確認|
|vagrant sandbox commit	|sandboxの現在の状態を保存(※非常に遅いので注意！)|
|vagrant sandbox rollback	|sandboxの状態を元に戻す|











## 共有フォルダの設定
Vagrantfileに以下のような設定を行うことでフォルダ共有が可能
下の設定では
ホスト側の./data(Vagrantfileがある場所がルート
と
ゲスト側の/home/vagrant/data
が共有している。

  config.vm.synced_folder "./data", "/home/vagrant/data", type: "nfs"


