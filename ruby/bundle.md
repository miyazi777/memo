概要
------------------------------
Gemを管理する為のツール。
インストールするgemはGemfileというファイルで管理する

コマンド
------------------------------

| コマンド                            | 内容                                         |
|:------------------------------------|:---------------------------------------------|
| bundle install                      | Gemfileの記述に沿ってgemをインストールする   |
| bundle install --path vendor/bundle | Gemfileの記述に沿ってgemをインストールする   |
| bundle update                       | Gemfileに沿ってgemを最新のものに更新する     |
| bundle exec <command>               | 現在のプロジェクト環境内でコマンドを実行する |

設定ファイル
------------------------------
<project-root>/.bundle/config

