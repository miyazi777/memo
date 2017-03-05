個人的なまとめ
======================================
個人的なrailsに関するメモです。

公式リファレンス
--------------------------------------
http://api.rubyonrails.org/

helper
--------------------------------------

## カスタムヘルパーについて
app/helpers/配下で定義すること


generateコマンド
--------------------------------------

### モデル
rails g model <model-name(単数形)> <item-name>:<data-type>

```
ralls g model Author name:string age:integer

```

#### 外部キー制約月
以下のやり方でauthor_idが絡むに追加される

```
rails g model Book title:string author:references
```

### コントローラ
以下のコマンドでコントローラの雛形を生成。コントローラ名が複数形か単数形かはケースバイケース

rails g controller <contoller-name> [<action-name> <action-name>]

```
rails g controller Books
```

#### 生成メソッドを一部に絞る
以下の例ではindexとshowのみ作成される

```
rails g controller Books index show
```

### マイグレーション
rails g migration <db-name(単数形)> <item-name>:<data-type>
基本的にはmodelと同じだが、こちらはモデルファイルを作成しない。

### scaffold
rails g scaffold <db-name(単数形)> <item-name>:<data-type>
基本的にはmodelと同じだが、こちらはコントローラ・モデル・マイグレーションファイルなど、全て作成する

### 上記のやり方で生成したファイルをなどを削除したい時
以下のコマンドで生成した場合
```
rails g controller clock new
```

以下のコマンドで削除できる
```
rails g destory controller clock new
```

routing
--------------------------------------
URLとコントローラとのマッピングはconfig/routes.rbにて定義されている。

### ルーティングの確認方法
コマンドラインから以下のコマンドで確認可能
```
rake routes
```

#### webブラウザで確認する方法
/rails/info/routesにアクセスするとルーティングが確認できる

### resources
resource :<resouce-name>をroutes.rbに記述することで規約に沿ったルーティングを自動生成する。
例えば、以下のように記述するとprojectsに関するルーティングが自動生成される。

```
resource :projects
```
自動生成されるルーティング
```
       Prefix Verb   URI Pattern                                      Controller#Action
     projects GET    /projects(.:format)                              projects#index
              POST   /projects(.:format)                              projects#create
  new_project GET    /projects/new(.:format)                          projects#new
 edit_project GET    /projects/:id/edit(.:format)                     projects#edit
      project GET    /projects/:id(.:format)                          projects#show
              PATCH  /projects/:id(.:format)                          projects#update
              PUT    /projects/:id(.:format)                          projects#update
              DELETE /projects/:id(.:format)                          projects#destroy
```

### rootにマッピング
root(/となるURL)にマッピングするには以下のような設定をroutes.configに追記
```
root 'projects#index'
```

### 独自にマッピング
独自にマッピングを定義することも可能。
以下のような書式で定義する。

<http-method> 'url' => '<controller>#<method>'

以下は/projects/<project-id>/takss/<id>/toggleというurlにtasksController#toggleメソッドが対応している設定。
```
  post '/projects/:project_id/tasks/:id/toggle' => 'tasks#toggle'
```









model
--------------------------------------

### validation

#### 基本
項目名, バリデーションの種類を指定することでバリデーションを設定する
必須チェックを追加する例
```
class Project < ActiveRecord::Base
    validates :title,
        presence: { message: "入力してください" }
end
```

#### バリデーションのヘルパー
:allow_nil      対象の値がnilの場合にはバリデーションをスキップ
:allow_blank    対象の値がblank? => trueの場合にバリデーションをスキップ
:on             バリデーションの実行のタイミングを設定。:createなどを指定するとその場合だけバリデーションが実行される。
:message        バリデーションのエラーメッセージ。設定しない場合はデフォルトのエラーメッセージを表示される。

#### 各種バリデーション

##### 空の時、エラー
```
validates :title, presence: true
```

##### 長さのチェック
```
validates :title, length: { minimum: 1}     # １文字以上
validates :title, length: { maximum: 75}    # 75文字以下
validates :title, length: { in: 1..75}      # １文字以上75文字以下
validates :title, length: { is: 8}          # 8文字
```

#### 条件付バリデーション
あとで記述


view
--------------------------------------

## 組み込みrubyの記法(erb)

出力がいらないコード
```
<% ~ %>
```

結果を出力
```
<%= ~ %>
```

結果をエスケープしないで出力
```
<%== ~ %>
```

後ろの改行を取り除く
```
<% ~ -%>
```

行頭までの空白を削除
```
<%- ~ %>
```

コメント
```
<%# ~ %>
```




## helper

### フォームの作成

#### エラーメッセージの表示
以下のソースの１行目でエラーの有無を判定し、２行目でエラーがあれば、バリデーションで設定されたメッセージを表示する
```
  <% if @project.errors.any? %>
  <%= @project.errors.messages[:title][0] %>
  <% end %>
```

#### viewのform_forで生成されるurlについて
form_forに指定するモデルの状態を判別してURLを自動で生成する。
form_for(@book)で@bookが空の状態であれば、action="/books"。
form_for(@book)で@bookに何かのモデルで入っていれば、action="/books/:id"。

### ラベル
```
link_to(name, options, html_options, &block)
```

#### 例
```
link_to "Profile", profile_path(@profile)
# => <a href="/profiles/1">Profile</a>
```

### submitボタン
```
<%= form_for @project do |f| %>
    <%= f.submit %>
<%= f.submit %>
```

### テキストフィールド
```
<%= f.text_field :title %><br>
```

### セレクトボックス
```
<%= f.collection_select(:dept_id, @dept, :id, :name) %>
<%= collection_select(:user, :dept_id, @dept, :id, :name) %>
```


## 共通のviewについて
共通の記述がある場合は別ファイルに切り出して、共通部品として使用することができる。

### 共有パーシャルについて
全体で共有したいviewのパーシャルについては/app/views/sharedに配置するのがrailsの慣例とのこと。

### 例
以下の例では共通部品_form.html.erbをnew.html.erbから使用している。

_form.html.erb
```
<p>
    <%= f.submit %>
</p>
```

new.html.erb
```
    <%= render 'form' %>
```

tips
--------------------------------------

### rails newする時にbundle installしない
Gemfileが変更になることがわかりきっている場合はbundle installを実行しない方が時間が節約できる
```
rails new <app-name> --skip-bundle
```

### 開発用のデータを作成する
おおまかな手順は以下のとおり。
* db/seeds.rbファイルにrubyのコードを追記
* bundle exec rake db:migrate:resetでDBのデータを削除
* bundle exec rake db:seed

以下にseed.rbの内容を示す。
```
dept1 = Dept.create(name: 'dept1')

User.create(name: 'user1', dept: dept1)
User.create(name: 'user2', dept: dept1)
User.create(name: 'user3', dept: dept1)
```
### 開発用のデータを作成する(その２）& テスト用データ
* test/fixtures/xxx.ymlにテスト用データを記述する
* bundle exec rake db:fixtures:loadでDBにデータを設定する

以下にfixtureのデータを示す(test/fixtures/zips.yml
```
one:
  zip_code: 100-1701
  address: 東京都青ヶ島村

two:
  zip_code: 100-0301
  address: 東京都利島村
```

### viewsに渡されたオブジェクトの内部データを表示
以下のようなコードを記述することで内部にどのようなデータを保持するか表示することができる
```
  <%= @project.errors.inspect %>
```

### railsデバック（controller)

#### 標準機能(ruby-debug)
* コード上のデバックしたい箇所にdebuggerを追記
* ブラウザでデバックしたい箇所にアクセスする
* railsが起動しているコンソール上で(byebug)と表示される。ここでrailsコンソールのように変数の値などを確認することができる
* Ctrl+Dで終了することができる

##### 用意されているコマンド
| コマンド     | 内容                                   |
|--------------|----------------------------------------|
| s - step     | 処理を進める(ステップイン)             |
| n - next     | 処理を進める(ステップアウト)           |
| c - continue | 処理を進める(ブレイクポイントで止まる) |

上記以外の詳細はこのサイトにある
http://bashdb.sourceforge.net/ruby-debug.html#Debugger-Command-Reference


#### rails-pryを使ったやり方
##### 必要なGem
```
gem 'pry-rails'
gem 'pry-byebug'
```

##### デバック方法
* コード上のデバックしたい箇所にbinding.pryを追記
* ブラウザでデバックしたい箇所にアクセスする
* railsが起動しているコンソールが入力可能状態となる。ここでいろいろなコマンドを打ち込むことでデバック可能となる。コマンドは以下の表を参照。
* exitと打ち込むと終了することができる

| コマンド             | 内容                         |
|----------------------|------------------------------|
| exit                 | デバック終了                 |
| next                 | 次の行へ移動                 |
| step                 | ???                          |
| p <変数名>           | 変数の内容を表示             |
| pp <変数名>          | 変数の内容を表示             |
| <クラス名>.inspect   | オブジェクトの情報を表示     |
| <クラス名>.methods   | オブジェクトの情報を表示     |
| <クラス名>.ancestors | オブジェクトの継承関係を表示 |

### railsデバック(view)
以下のようなコードでビューの中身を知ることができる

#### 変数の中身を表示
```
<%= debug @test %>
```

#### 変数の中身を表示(yaml)
```
<%= @test.to_yaml %>
```

#### 変数の中身を表示(inspect)
```
<%= @test.inspect %>
```

### 使用しているミドルウェアの確認

```
rake middleware
```

### bundle exec rake xxxについて
rails 4.1まではbundle exec rake xxxだったが、rails 4.2以降はbin/rake xxxで良いらしい

### bundle installする際のtips
bundle installする時、--path vendor/bundle --jobs=4 を指定すると速く終わる。

#### --path vendor/bundleについて
この指定はgemのインストール先を指定する際に必要。このように指定おくとrailsプロジェクト毎にgemを管理できる、というメリットがある

#### --jobs=nについて
bundle installの並列数を指定することができる

### アセットパイプラインの設定を変更する場合
/config/initializers/assets.rbにアセットパイプラインに含めるパスなどを設定すること

### railsのmailerでgmailにメール送信する際に送信できた設定
```
config/environments/development.rb
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.smtp_settings = {
    :enable_starttls_auto => true,
    :address => 'smtp.gmail.com',
    :port => 587,
    :domain => 'gmail.com',
    :authentication => 'plain',
    :user_name => 'm71203',
    :password => 'password'      # ここだけ２段階認証で生成されたパスワードを設定すること
  }
```

他のコントローラから呼び出すのは以下のようなコード
```
class TestController < ApplicationController
  def index
    NoticeMailer.sendmail_confirm.deliver
  end
end
```

以下はaws sesの設定例
```
config/environments/development.rb
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.smtp_settings = {
    :enable_starttls_auto => true,
    :address => 'email-smtp.us-east-1.amazonaws.com',           # SMTP settingのserver name
    :port => 587,                                               # SMTP settingのport
    :domain => 'gmail.com',
    :authentication => 'login',
    :user_name => '<aws-credential>',                       # SesSendingAccessしかないユーザー access_key
    :password => '<aws-secret>' # SesSendingAccessしかないユーザー secret_key
  }
```

### ActiveRecordの保存時にバリデーションを行わないようにする
以下のようなコードで可能
```
model_instance.save!(validate: false)
```

### railsでJSONのエンコードとデコード
active_supportを使う
```
require 'active_support/core_ext/object/json.rb'

class TestsController < ApplicationController
  def index
    puts "----------------------------------------------------"
    data = {
            "id" => 5,
            "name" => "tama2",
            "item1" => "value1",
            "item2" =>
                  [
                  {
                    "item2-1" => "value2-1",
                    "item2-2" => "value2-2",
                    "item2-3" => "value2-3"
                  },
                  {
                    "item2-4" => "value2-4",
                    "item2-5" => "value2-5"
                  }
                  ],
            "item3" => {
                    "item3-1" => "value3-1",
                    "item3-2" => "value3-2",
                  }

           }
    json = ActiveSupport::JSON.encode(data)   # ここでハッシュからJSON文字列へ変換
    puts json
    puts "----------------------------------------------------"
    obj = ActiveSupport::JSON.decode(json)    # ここでJSON文字列からrubyオブジェクト(ハッシュ)へ変換
    puts obj
    puts "----------------------------------------------------"
    puts "name = #{obj['name']}"
    puts "item3.item3-1 = #{obj['item3']['item3-1']}"
    puts "----------------------------------------------------"
  end
end
```

### fogの使い方
#### S3との連携
##### ファイルのアップロード
```
    storage = Fog::Storage.new({
      provider: 'AWS',
      aws_access_key_id: 'AKIAJJ7DWXGUH4RPHJAQ',
      aws_secret_access_key: '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',
      region: 'ap-northeast-1'
    })
    bucket = storage.directories.get('cdb-user-assets-dev')
    file = bucket.files.create(key: 'testtest.txt', public: true, body: 'test text')
    puts file.public_url
```

##### ファイルのダウンロード
```
    bucket = storage.directories.get('cdb-user-assets-dev')
    file = bucket.files.get('testtest.txt')
```

##### ファイルコピー
以下の例ではtesttest3.txtからtesttest5.txtへコピーしている
```
    storage.copy_object('cdb-user-assets-dev', 'testtest3.txt', 'cdb-user-assets-dev', 'testtest5.txt')
```

##### ファイル削除
```
    bucket.files.destroy('test_file')
```

##### マウントされた項目を文字列（ファイル名）として取得
```
    src = order.template[:file]
```

## arでのleft outer joinのサンプル
```
    Order.joins(:template).joins('left outer join `jobs` on `jobs`.`id` = `orders`.`job_id`').where('orders.template_id = 1').select('orders.id as id, jobs.name as name, templates.name as tname')
```

以下のような指定の場合、referencesでouter joinしたいものを明示的に指定する必要がある。
```
    x = Order.joins(:template).includes(:job).group('orders.order_group_id').where('orders.template_id = 1').references(:job).select('orders.id as id, jobs.name as job_name')
    x[0].job.name => もし、jobがあれば、nameを出す。なければ、jobはnilになる
```

## ransackでidなどのprimary keyをlike検索したい場合の対応
以下のようにprimary_keyをlike検索したい場合は対象のmodelに以下のような対応を加えることで文字列に変換後のlike対象とすることができる

view
```
{"id_cont"=>"99"}
```

model
```
  private
  # rancak用の対応
  ransacker :id do
    Arel::Nodes::SqlLiteral.new("CONVERT(#{table_name}.id, CHAR)")
  end
```

参考:
https://github.com/activerecord-hackery/ransack/issues/85

## docker-compose.ymlでpryを使う方法

(1)
```
  app:
    tty: true
    stdin_open: true
    build:
      context: .
      args:
        workdir: /codenberg
    command: rails s -b 0.0.0.0
    volumes:
      - .:/codenberg
    ports:
      - 3000:3000
    links:
      - db
      - cache
    volumes_from:
      - bundle
```

(2) binding.pryをコードのどこかに記述

(3) attachする
```
docker attach <アプリ用のコンテナ名>
```

(4) 抜ける時はctrl-p, ctrl-q

## active recordでサブクエリでcountをとる

```
      order_groups = OrderGroup.joins(:orders => :template)
          .where(account_id: @current_account.id).group('order_groups.id')
          .select('order_groups.*, count(orders.id) as order_count, sum(orders.delivery_number) as order_delivery_number, templates.name as template_name')
      count = OrderGroup.from(order_groups).count
```


# railsでip指定でアクセス制限をかける


config/application.rb

```
    config.middleware.use Rack::Access, {
        "/factory" => ["127.0.0.1", "153.156.71.2"],
        "/system_manage" => ["127.0.0.1", "153.156.71.2"],
    }
```


# 設定ファイルを指定してsidekiqを起動する方法
```
bundle exec sidekiq -C config/sidekiq.yml
```

# 絵文字・機種依存文字・サロゲートペアの判定方法
```
MACHINE_DEPENDENT_CHARS_REGEX = /[\u{2150}-\u{219f}\u{2460}-\u{24ef}\u{2600}-\u{2660}\u{3220}-\u{325b}\u{3280}-\u{33ff}]/.freeze

EMOJI_REGEX = /[\u{00A9 00AE 203C 2049 2122 2139 2194}-\u{2199 21A9}-\u{21AA 231A}-\u{231B 2328 23CF 23E9}-\u{23F3 23F8}-\u{23FA 24C2 25AA}-\u{25AB 25B6 25C0 25FB}-\u{25FE 2600}-\u{2604 260E 2611 2614}-\u{2615 2618 261D 2620 2622}-\u{2623 2626 262A 262E}-\u{262F 2638}-\u{263A 2648}-\u{2653 2660 2663 2665}-\u{2666 2668 267B 267F 2692}-\u{2694 2696}-\u{2697 2699 269B}-\u{269C 26A0}-\u{26A1 26AA}-\u{26AB 26B0}-\u{26B1 26BD}-\u{26BE 26C4}-\u{26C5 26C8 26CE}-\u{26CF 26D1 26D3}-\u{26D4 26E9}-\u{26EA 26F0}-\u{26F5 26F7}-\u{26FA 26FD 2702 2705 2708}-\u{270D 270F 2712 2714 2716 271D 2721 2728 2733}-\u{2734 2744 2747 274C 274E 2753}-\u{2755 2757 2763}-\u{2764 2795}-\u{2797 27A1 27B0 27BF 2934}-\u{2935 2B05}-\u{2B07 2B1B}-\u{2B1C 2B50 2B55 3030 303D 3297 3299 1F004 1F0CF 1F170}-\u{1F171 1F17E}-\u{1F17F 1F18E 1F191}-\u{1F19A 1F201}-\u{1F202 1F21A 1F22F 1F232}-\u{1F23A 1F250}-\u{1F251 1F300}-\u{1F321 1F324}-\u{1F393 1F396}-\u{1F397 1F399}-\u{1F39B 1F39E}-\u{1F3F0 1F3F3}-\u{1F3F5 1F3F7}-\u{1F4FD 1F4FF}-\u{1F53D 1F549}-\u{1F54E 1F550}-\u{1F567 1F56F}-\u{1F570 1F573}-\u{1F579 1F587 1F58A}-\u{1F58D 1F590 1F595}-\u{1F596 1F5A5 1F5A8 1F5B1}-\u{1F5B2 1F5BC 1F5C2}-\u{1F5C4 1F5D1}-\u{1F5D3 1F5DC}-\u{1F5DE 1F5E1 1F5E3 1F5EF 1F5F3 1F5FA}-\u{1F64F 1F680}-\u{1F6C5 1F6CB}-\u{1F6D0 1F6E0}-\u{1F6E5 1F6E9 1F6EB}-\u{1F6EC 1F6F0 1F6F3 1F910}-\u{1F918 1F980}-\u{1F984 1F9C0}]/.freeze



def is_unprintable_chars?(str)
  # サロゲートペア文字が混じっていたらエラーとする
  result, err_str = is_surrogate_pair_chars?(str)
  if result
    return result, err_str
  end

  # 絵文字が混じっていたらエラーとする
  result, err_str = is_emoji?(str)
  if result
    return result, err_str
  end

  # 機種依存文字が混じっていたらエラーとする
  result, err_str = is_machine_dependent_chars?(str)
  if result
    return result, err_str
  end

  return result, err_str
end

# 機種依存文字判定
def is_machine_dependent_chars?(str)
  err_str = []
  result = false

  match_str = str.match(MACHINE_DEPENDENT_CHARS_REGEX)
  unless match_str.nil?
    result = true;
    err_str.push(match_str)
  end

  return result, err_str
end

# 絵文字判定
def is_emoji?(str)
  err_str = []
  result = false

  match_str = str.match(EMOJI_REGEX)
  unless match_str.nil?
    result = true;
    err_str.push(match_str)
  end

  return result, err_str
end

# サロゲートペア文字判定
def is_surrogate_pair_chars?(str)
  err_str = []
  result = false

  ary = str.each_codepoint.map{|n| n.to_i}
  ary.each do |codepoint|
    puts codepoint.chr("UTF-8") + "   " + codepoint.to_s(16)  # TODO 必要ない
    if codepoint >= 0x10000
      err_str.push(codepoint.chr("UTF-8"))
      result = true
    end
  end
  return result, err_str
end

result, err_str = is_unprintable_chars?("xx義ほ🍺げほげ!の屋x")
puts "----- result -----"
puts result
puts err_str
```


## モデルにプロパティを追加し、jsonで出力する方法

モデル側
```
attr_accessor :unit, :order_value, :zzz, :xxx

def as_json(options={})
  super.as_json(options).merge({zzz: self.zzz, xxx: self.xxx})
end

```

コントローラ
```
  def get_custom_field
    @template = Template.find params[:id]
    @edit_field = TemplateCustomField.where('template_id=?', @template.id)
    @edit_field.each do |f|
      f.pos_x = f.pos_x.to_mm
      f.pos_y = f.pos_y.to_mm
      f.width = f.width.to_mm
      f.height = f.height.to_mm
      f.xxx = "test1"
      f.zzz = "test2"
    end
    return render json: {status: 'success', edit_field: @edit_field}
  end
```


# railsのタイムゾーンでTimeクラスを初期化する方法

```
Time.zone.local(d.year, d.month, d.day)
```

### active recordでわざわざメソッドを作らなくても良いケース
find_by_<項目名>とすると検索可能


## railsのクラスのオートロードパスについて

### デフォルト
* appのサブディレクトリ全て
* app/*/concernsディレクトリ
* test/mailers/previews

### パスの追加
config/application.rbで以下のようにする。以下はlibを追加する例。
```
config.autoload_paths << "#{Rails.root}/lib"
```

### autoload_pathsの確認
rails r 'puts ActiveSupport::Dependencies.autoload_paths'

## サブクエリの作り方

```
    ids = Order.where(id: [1, 2, 3]).select(:order_group_id)
    OrderGroup.where(id: ids)
```


## modelにpermitで更新データだけを設定する方法

```
 @calendar.attributes = calendar_params

 def calendar_params
   params.require(:factory_calendar).permit(:start_at, :end_at, :delivery_scheduled_at)
 end
```




## rakeタスクを実行する際に環境を指定する
```
export RAILS_ENV=production;
bundle exec rake ...
```

または以下のコマンドで実行
```
bundle exec rake .... RAILS_ENV="preview"
```



draper gemの使い方
===========================
## 概要
activerecordの検索結果を拡張する為のgem。
viewの表示の為にmodelに表示用のメソッドを追加することがあるが、それを別のクラスに分けるなどの使い方ができる。

## インストール
gem 'draper'

## 使い方
activerecordの結果にdecorateを付けるだけでDecoratorクラスに定義されたメソッドも使えるようになる


app/controller
```
@users = User.all.order(created_at: :desc).decorate
```

app/decorator
```
class UserDecorator < Draper::Decorator
  delegate_all      # 元のUserクラスのメソッドを使う為に必要

  def deco(checked_at)
    object.name + " xxx"
  end
end
```

view
```
@users.each do |user|
  user.deco
```

## decoreateするとkaminari用のメソッドが使えなくなる問題への対応方法
以下のような設定ファイルを定義すると

/config/initializers/draper.rb
```
Draper::CollectionDecorator.delegate :current_page, :total_pages, :limit_value, :total_count
```

### 参考
http://tnakamura.hatenablog.com/entry/2014/09/01/123000
https://github.com/drapergem/draper/issues/401




railsのcontroller意外でパスを取得する方法
===========================

## ドメイン取得
Rails.application.config.environment_domain
=> "http://localhost:3000/"

## コントローラ名とアクション名からパスを取得
Rails.application.routes.generate_extras({:controller=>"users", :action=>"show", :id=>"1"})
=> ["/users/1", []]
Rails.application.routes.generate_extras({:controller=>"orders", :action=>"group_show", :id=>"1"})

## URLからコントローラとアクションを特定する
Rails.application.routes.recognize_path "users/1"
=> {:controller=>"users", :action=>"show", :id=>"1"}


コントローラ内でハッシュからjsonを生成する
===========================
ActiveSupport::JSON.encode(order.to___json)   # ここでハッシュからJSON文字列へ変換


ハッシュのキーをシンボル、文字列に相互変換
===========================
## シンボル化
```
{'a' => 1, 'b' => 2}.symbolize_keys
# => {:a=>1, :b=>2}
```

## 文字列化
```
{:a => 1, :b => 2}.stringify_keys
# => {"a"=>1, "b"=>2}
```

定数へのアクセス
===========================

以下のようなクラスが存在する時
```
class Hoge
  ABC = "aaa"
end
```

以下のようなコードでアクセスできる

```
Hoge::ABC
```



circle ciに関して
===========================

## テスト失敗時のノウハウ
### 他環境へのアクセスでテスト失敗
基本的には他の環境(sidekiqへのエンキューやslackへの通知など)へのアクセスが発生する環境ではエラーとなる確率が高い。
その為、他環境へアクセスする部分はreturn if Rails.env.test?などでテスト環境では動作しないようにする必要がある。
また、テストコードでうまく呼び出し部分をモックにすること。




rubyの特殊な記法、演算子、イディオムのまとめ
===========================
http://qiita.com/nashirox/items/0c885edf7d78fd5a83f1



