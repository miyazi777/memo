
-----------------------------------------------------------

* shell    ash
* パッケージマネージャ    apk


## dockerでコンテナ立ち上げる
d run -it --name test alipine ash

### apkコマンド
apk update  パッケージリポジトリから最新のインデックスを取得
apk search <package-name>   パッケージの検索
apk add <package-name>      パッケージのインストール
    --no-cache              キャッシュファイルを残さないオプション

