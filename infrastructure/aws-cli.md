
## インストール
pip install awscli

### 補完設定
zshを使っている場合は以下の設定を.zshrcに追加することで補完されるようになる
```
source /usr/local/bin/aws_zsh_completer.sh
```


## cli用設定ファイル

~/.aws/config       設定が格納されるファイル
~/.aws/credentials  アクセスキーが格納されるファイル


## 設定追加コマンド
aws configure --profile <config-name>

```
aws configure --profile tasknotes
AWS Access Key ID [None]: xxxxxxxxxx
AWS Secret Access Key [None]: xxxxxxxxxx
Default region name [None]: ap-northeast-1
Default output format [None]: json
```

## cliの設定を指定してコマンドを実行する
以下はs3のバケット一覧を取得するコマンドだが、--profile <config-name>を指定することで接続先を変更することができる

```
aws s3 ls --profile fridaynaight
```
