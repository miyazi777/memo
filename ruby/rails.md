概要
=====================================
## rvenv
ruby言語自体のバージョン切り替えツール

## gemとは
ruby用のライブラリのこと。

## RubyGemsとは
ruby用ライブラリのリポジトリ。maven centralみたいなもの。

## bundlerとは
パッケージ(gem)管理ツール

## rakeとは
ruby用のビルドツール。makeのruby版。


rails系のドキュメント
=====================================
http://railsdoc.com/view


rails系のコマンド
=====================================

基本的にはこちらのサイトにまとまっている
http://railsdoc.com/rails#%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E3%81%A7%E3%82%B5%E3%83%BC%E3%83%90%E3%82%92%E8%B5%B7%E5%8B%95(rails%20server)

## rails new xxx   新しいrailsプロジェクトを作成。xxxはプロジェクトの名前。
### -B, --skip-bundleにてbundle installを行わない

## rails server    rails用のサーバーを起動 -b オプションでバインドするIPアドレスを指定。例: rails server -b 0.0.0.0
### rails s        上記の省略形
## rails generate scaffold xxx 雛形を作成

## rails db         DBに接続する
## rails console    インタラクティブシェルの起動


rakeのコマンド
=====================================

rake db:migrate DBへの反映
rake routes     ルーティングの確認


railsのアプリのディレクトリ構造
=====================================

## よくいじるもの(by dotinstall)

## app
### model       モデル
### controller  コントローラ
### views       ビュー
#### layouts    レイアウト用のビュー
## config
## db






# 手作業でプロジェクトを作成する場合の手順
例として以下のようなアプリを想定(by dotinstall)

Projectごとにタスクを管理するアプリ

## モデルを作成(ここではProjectのモデルを作成)
rails g model Project title:string
rake db:migrate

### 作成したモデルの確認
rails db    DBに接続する

### 作成したモデルの機能をインタラクティブに確認する
rails console

## コントローラを作成
rails g controller Projects

### viewを作成
app/views/projects/index.html.erbを以下のようにコーディング

<ul>
 <% @projects.each do |project| %>
   <li><%= project.title %></li>
 <% end %>
</ul>

これでprojectテーブルの内容が全て一覧表示されるハズ


## インテグレーションテストの生成
rails g integration_test <test-template-name>


## 4/10 memo

### scaffoldの例
以下のコマンドでUserデータのscaffoldを作成
bundle exec rails generate scaffold User name:string score:integer
bundle exec rails g scaffold User name:string score:integerでも可

以下のコマンドでUserデータをDBに反映
rake db:migrate

以下のURLにアクセスするとUserデータのCRUDが作成されている
<domain>/users

## 4/11 memo
### railsサーバーが起動しない時の対処方法
#### 原因
tmp/pids/server.pidが残っていることがある
#### 対応方法
tmp/pids/server.pidを削除する

dotinstallのメモ
-------------------------------------

## modelの作り方
### モデルそのものを作成
bundle exec rails g model <model-name> <item-name>:<data-type>

bundle exec rails g model Project title:string

* model-nameは単数系で最初は大文字を指定
* このコマンドを実行するとテーブル定義が/db/migrate下に作成される

### DBへの反映
bundle exec raike db:migrate

このコマンドを実行することで/db/migrateファイル下の定義に沿ってテーブルが作成される

### データの追加
bundle exec raike console

Project.create(title:"p1")
Project.all
quit



## controllerの作り方
### コントローラの生成
bundle exec rails g controller <model-name>

* model-nameは複数で最初は大文字

実際には下のようなコマンドを打ち込んだ。
docker exec -it dotinstall_web_1 bundle exec rails g controller Projects

### ルーティングの生成
config/routes.rbに以下のような記述を追加
```
 resources :projects
```

### ルーテイングの確認
bundle exec rake routes

### before_actionとafter_action
before_action   各コントローラのメソッドが実行される前に行われる処理を定義可能
after_action    各コントローラのメソッドが実行される後に行われる処理を定義可能


## DBツール
### rails consoleの起動方法
docker上の開発ツールでは以下の形でrails consoleが使える
docker exec -it <containar-name> bundle exec rails console

### rails consoleの使い方
データの追加
p = Project.new(title: "p1")
p.save

データの追加2
Project.create(title: "p2")

データの確認
p

データの全権確認
project.all



## 疑問点
docker環境でrails dbでDB操作はどうやってやるのか？
-> ホスト上ではうまくできないが、ゲストOSに接続した後にrails consoleをやるとうまく行く

## helper

image_tag <src-path>    imageタグを生成
link_to <text>, <url>   linkタグを生成。urlはrake routesで表示されるprefixの書き方も大丈夫
form_for                各種フォームを生成


4/14
--------
rails new <app-name>    railsアプリを新しく生成する
rails g controler <controller-name> <method>    contollerを生成
rails g scaffold <data-name> <data-types>       指定されたdata-nameのデータのCRUDを作成する
rails d contoller <contolller-name> <method>    generateで作ったcontrollerを削除

rake routes         ルーティングを確認
rake db:migrate     db/migrateファイル下の定義に沿ってDBにテーブルが作成される


4/17
--------
テスト(minitest)の実行方法

```
bundle exec rake test
```


未整理のメモ
=====================================
