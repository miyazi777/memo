
# ショートカット
ウィンドウの分割        ショートカットなし
ウィンドウの移動        alt + tab   or alt + tab + shift
タブの移動              cmd + @  or cmd + [
クラス名指定で検索      cmd + o
ファイル名指定で検索    cmd +shift + o
メソッドジャンプ        ctrl + [
メソッドジャンプを戻る  ctrl + o
呼び出し元表示          alt + F7



# 設定ファイル
~/Library/Preferences/RubyMine2016.1/idea.vmoptions


# リモートデバックの手順

## Gemfileの修正
ruby-debug-ide

### 追加

## rubymine側の設定
Run -> Edit Configurations
左上の+でRuby remote debugを追加

Remote host: 192.168.99.100
Remote port: 1234
Remote root folder: /codenberg
Local port: 26162
Local root foler: /User/miyajimatakeshi/fridaynight/work3/codenberg


## docker-compose.ymlを修正
command: bundle exec rdebug-ide --host 0.0.0.0 --port 1234 --dispatcher-port 26162 -- bin/rails s -b 0.0.0.0

### 1234番ポートも開ける
    ports:
      - 1234:1234
      - 3000:3000

## docker起動
1234ポートで待ち受けしているメッセージが表示される

## rubymine側のデバッカを起動


## あとはrubymine側のソースにブレイクポイントを追加し、ブラウザからアクセスする

## ブレイクポイントの箇所でnilエラーが起こる場合
byebug gemが原因。Gemfileに書かれていなくてももともと入っていることがあるのでbudle cleanなどしてbyebugを削除
pry-byebugも同様

ちなみに入れていると以下のメッセージが出る

```
NoMethodError (undefined method `+' for nil:NilClass):
```
