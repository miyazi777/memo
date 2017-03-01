# install
gem install pry pry-doc
pry -v

# hello world

<pre>
pry
> puts "Hello World"
</pre>


## 終了コマンド
!!!

### 簡易コマンド表

|command|desc|
|:--|:--|
|cd|オブジェクトの移動|
|ls|オブジェクトのメソッドや変数の表示|
|show-source <method name>|メソッドを表示|
|edit|エディタでソースをを表示|
|edit-method|メソッドのファイルと行数がわかる？|
|変数名|オブジェクトの内容を確認|
|hist|コマンドの履歴の表示|
|!!!|pryの終了|

# その他

* pry-navというgemを使うことでステップ実行できる？


## 単体のrubyスクリプトのデバック方法

以下のようなサンプルスクリプトのデバック
```
def test
  puts "test"
  x = 7
  binding.pry
  if x == 7
    puts "seven"
  end
end
```

### pry起動
pry

### 対象ファイルをロードする
load 'test.rb'

### 実行
Test.new.test


