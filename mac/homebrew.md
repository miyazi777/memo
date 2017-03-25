# home brew

## コマンド

|動作|コマンド|
|---|---|
|インストール済みのフォーミュラをリスト表示|brew list|
|インストール済みのフォーミュラの情報を更新|brew update|
|インストール済みのフォーミュラを更新|brew upgrate <fomura-name>|


# brew cask
==========

## homebrew-caskとは
homebrewの拡張。どの部分が拡張なのかはよくわからないが、dmgファイルでしか提供されていない、homebrewだけでは導入できないアプリもコマンドライン経由でインストールできる仕組みを提供するのが目的っぽい。


## インストール方法
brew install caskroom/cask/brew-cask


## cask経由でのアプリインストール方法
brew cask install <アプリ名>



brew updateでエラーが出た時の対応
=========

## ライブラリのディレクトリの権限を変更
パーミッションがどうのこうのと出ていたので、

* sudo chown -R <user> /usr/local/Library


## brewの最新化
* cd $(brew --prefix)
* git fetch origin
* git reset --hard origin/master
* brew cleanup --force
* brew update


## 参照サイト
[http://stackoverflow.com/questions/14113427/brew-update-failed]



