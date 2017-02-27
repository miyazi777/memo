# 概要
rubyのバージョンを切り替える為のツール。
基本には2.2.2だけど、あるディレクトリ以下だけ1.9.3にしたい、みたいなことができる。



## command list
| rbenv install --list | インストール可能なrubyのバージョンリストを表示                                     |
| rbenv install xxx    | xxxで指定したバージョンのrubyをインストール                                        |
| rbenv versions       | インストール済みのrubyのバージョン一覧を表示                                       |
| rbenv global xxx     | xxxで指定したバージョンのrubyへ切り替え                                            |
| rbenv local xxx      | xxxで指定したバージョンのrubyへ切り替え(現在のディレクトリのみ)                    |
| rbenv rehash         | rbenvでgemをインストールした時、シンボリックリンクを更新する必要があり、それを行う |

# install(mac)

## rbenv自体のインストール

brew install rbenv

## rbenvの設定(zsh)

export PATH="$HOME/.rbenv/bin:$PATH"
export PATH="$HOME/.rbenv/shims:$PATH"
eval "$(rbenv init - zsh)"

## rbenv install -lのリストを更新する

rbenv installはruby-buildによって提供されている為、ruby-buildを更新する

```
ruby-build --version
brew update
brew upgrade ruby-build
ruby-build --version
```

設定ファイルなど
----------------------

~/.rbenv以下

rbenv localでディレクトリごとのrubyバージョンを指定した場合はそのディレクトリに.ruby-versionファイルが作成される

## 例
~/test以下をruby 2.3.1にしたい時
```
rbenv local 2.3.1
```



# plugin

### ruby-binstubs
bundle execを省略する為のプラグイン

