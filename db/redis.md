# クラスタの構築方法

## 前提
virtualboxとvagrantがインストールされていることが前提。
3台のvagrant上に構築する。

os : debian 7.2
node1 : 192.168.33.71
node2 : 192.168.33.72
node3 : 192.168.33.73
redisのポート : 6379

## vagrant上での構築方法
VagrantFileに以下のように記述。
box名は自分の環境のbox名に合わせてください。

<pre>
config.vm.box = "debian72"

config.vm.define :redis1 do |node|
  node.vm.network :private_network, ip: "192.168.33.71"
end

config.vm.define :redis2 do |node|
  node.vm.network :private_network, ip: "192.168.33.72"
end

config.vm.define :redis3 do |node|
  node.vm.network :private_network, ip: "192.168.33.73"
end
</pre>

## インストール方法
wget http://download.s.io/releases/redis-3.0.2.tar.gz
tar xzf redis-3.0.2.tar.gz
cd redis-3.0.2
make

## クラスタ化のための設定
~/redis-3.0.2/src/redis.confの最後に以下の設定を追加する

<pre>
### custer setting
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
</pre>


## redisサーバ起動
~/redis-3.0.2/src/redis-server redis.conf

### 動作確認
これでbarが表示されれば、動作OK

</pre>
~/redis-3.0.2/src/redis-cli
set foo bar
get foo
</pre>

## gemのインストール
これを入れておかないとクラスタ起動時にエラーが出てことがある。

<pre>
sudo gem install redis
</pre.


## クラスタ作成
<pre>
./src/redis-trib.rb create 192.168.33.71:6379 192.168.33.72:6379 192.168.33.73:6379
</pre>

### gemがなくてエラーが出た時
下のようなエラーが発生した場合はこのコマンドでredis用のgemをインストールする必要がある。

<pre>
sudo gem install redis
</pre>

<pre>
/usr/local/lib/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- redis (LoadError)
	from /usr/local/lib/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
    from ./src/redis-trib.rb:25:in `<main>'
    vagrant@debian-7:~/redis-3.0.2$ cd /usr/local/lib/
</pre>


## クライアント起動
-cオプションをつけて起動すること。
これでどのノードから起動してもtestkeyが取得できるはず。

<pre>
~/redis-3.0.2/src/redis-cli -c
set testkey testvalue
</pre>


## その他

### クラスタには最低3台必要
redisクラスタの構築には最低３台のredisが必要。３台ない場合はクラスタ起動時に怒られる。


### データのレプリカ
上の手順だとレプリカは作らない(レプリカ数は0)。レプリカを指定して複数ノードに同じデータを保持したい時にはredis-trib createコマンドのオプションでreplicasを指定する。
レプリカを設定することでもし、特定のredisサーバが落ちてもデータが生き残る可能性が上がるが、メモリもその分、使うのでレプリカ数の設定は慎重に。


### 参考

http://redis.io/topics/cluster-tutorial




