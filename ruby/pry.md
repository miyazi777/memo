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





railsでのデバック
-------------------------------------

## pryで条件付きブレークポイント
以下をソース内に記述する
```
binding.pry if <expresion>
```

## pryの設定
<projectroot>/.pryrcに記述することで有効に

### 設定例

コマンドのエイリアス
```
# step command alias
if defined?(PryByebug)
  Pry.commands.alias_command 's', 'step'
  Pry.commands.alias_command 'n', 'next'
  Pry.commands.alias_command 'f', 'finish'
  Pry.commands.alias_command 'c', 'continue'
end
```

コマンドを押すだけで前のコマンドを繰り返せる設定
```
# Hit Enter to repeat last command
Pry::Commands.command /^$/, "repeat last command" do
  _pry_.run_command Pry.history.to_a.last
end
```

editコマンドから起動するテキストエディタを選択可能
editコマンド__上からソースを編集すると再読込されている
```
Pry.config.editor = "vim"
```

## 参考
http://qiita.com/k0kubun/items/b118e9ccaef8707c4d9f






その他
-------------------------------------

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


