# マウスでのコピペ
alt + マウスの操作で可能

# コマンド

## セッション中の操作

| operation                        | shortcut                                |
|----------------------------------|-----------------------------------------|
| セッションの選択                 | prefix-s                                |
| セッション名の変更               | prefix-$                                |
| セッションのデタッチ             | prefix-d                                |
| ヘルプを参照                     | prefix-?    (ヘルプ画面を抜けるには???) |
| ウィンドウの切り替え             | prefix-NoKey                            |
| ウィンドウの切り替え             | prefix-n or prefix-b                    |
| ウィンドウの選択                 | prefix-w                                |
| ウィンドウの作成                 | prefix-c                                |
| ウィンドウの削除                 | prefix-&                                |
| ペイン分割（水平)                | prefix-"                                |
| ペイン分割（垂直)                | prefix-%                                |
| ペインの移動                     | prefix-q 表示された番号を指定           |
| ペインを削除                     | prefix-x                                |
| 各ペインのインジケータ表示       | prefix-q                                |
| 各ペインのインジケータ表示・移動 | prefix-q-NoKey                          |


## コマンドライン上の操作

| operation                  | shortcut                            |
|----------------------------|-------------------------------------|
| セッション名を指定して起動 | tmux new -s [session name]          |
| セッションの一覧を確認     | tmux ls                             |
| セッションアタッチ         | tmux a -t [session-name]            |
| セッションを削除           | tmux kill-session -t [session name] |
| セッションを全て削除       | tmux kill-server                    |


## コピーモード
| operation        | shortcut |
|------------------|----------|
| コピーモード開始 | prefix-[ |
| コピーの始点決定 | space    |
| コピーの終点決定 | enter    |





# カスタマイズについて

## 設定ファイル
.tmux.confファイルを作成しておくとtmux起動時に読み込んでくれる

### 変更した設定ファイルの反映方法
prefix-keyを押した後、: source-file ~/.tmux.conf

### もう１つ反映方法の方法
いずれかの方法で修正を反映していく

* tmuxを全て終了して、再度、tmuxを軌道する
* 設定ファイルに再起動用の設定を記述する







