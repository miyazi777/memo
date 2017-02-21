# ansible memo

## インストール
macならhome brewでインストール可能

<pre>
brew install ansible
</pre>


## パスワードを聞かれないようにする方法
### ssh関連の設定をsshの設定ファイルに登録
vagrant ssh-config >> ~/.ssh/config
ssh default
### defaultがいやな場合はdefaultを別の文字列位書き変える


## 疎通の手順
### hostsファイルを用意
echo testserver > hosts
ansible -i hosts testserver -m ping
### コマンド実行結果
testserver | success >> {
    "changed": false,
    "ping": "pong"
}
上のように表示されたらOK
## gitをplaybookでインストールするp
### playbookの作成
playbook.ymlという名前で以下のようなファイルを作成する

<pre>
---
- hosts: local-servers
  sudo: true
  tasks:
      - yum: name=git state=present
</pre>

### playbookの実行
ansible-playbook -i hosts playbook.yml




















## vagrant + ansibleでの大まかな作業の流れ

* vmを作成&起動
 * 適当に作業要ディレクトリを作成
 * vagrant initでVagrantfileのひな形を作成
 * Vagrantfileを修正し、vagrant upでvmを起動
* inventory fileの作成
 * inventory fileは制御対処のサーバのリストを記述するリストファイル
* playbookの作成
 * playbookでサーバに入り込んでから行う操作を記述したファイル。yml形式


## チュートリアル1 pingする

* 以下のコマンドで実行対象となるホストファイルを作成
 * echo "127.0.0.1" > hosts
* ansibleでpingコマンドを実行する
 * ansible all -i hosts -m ping




## 有名な日本語チュートリアルを試す
http://yteraoka.github.io/ansible-tutorial/#test-ansible

* 「2.Ansibleをインストールする」はチュートリアルどおりにxbuildがうまくいかずにpythonの最新版がインストールできなかったのでyumでansibleをインストール
 * yumにepelパッケージを追加
   * epelを追加
     * wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
   * remiを追加
     * wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
   * レポジトリをインストール
     * rpm --upgrade --verbose --hash epel-release-6-8.noarch.rpm  remi-release-6.rpm
 * yumでansibleをインストール
   * sudo yum install ansible



## ansibleでdockerコンテナを構成する方法
* dockerコンテナ内にpythonをインストールする
* playbook内でconnection: dockerを入れておく
[詳細はこのページを参照](http://www.indetail.co.jp/blog/8544/)

