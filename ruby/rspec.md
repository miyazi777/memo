RSpec memo
========================

install
------------------------
gem install rspec

起動方法
------------------------
railsプロジェクトルートで以下のコマンﾄﾞを実行

```
rspec
```

もし、特定のテストケースのみ実行したい場合はファイル名を指定

```
rspec spec/models/contact_spec.rb
```

rspecテストのgenerate
------------------------
rails g rspec:model order_group

## apiのテストケースを作成する場合
rails g rspec:integration orders
  -> spec/requests/order_spec.rb

## feature spec(capybaraなど)のテストケースを作成する場合
rails g rspec:feature factory_signin

4/18 memo
------------------------
初期化
rspec --init

すると以下のファイルが作成される。
.rspec                  デフォルトで実行されるコマンドオプション
spec/spec_helper.rb     設定ファイル？


以下のコードブロックで囲われた部分が１テスト
it "テストの内容分" do
  expect(評価式).to eq(5)
end

assertがexpect

beforeブロックが最初に１度だけ実行される処理を記述する箇所
before do
end

describeについて
テスト対象を指定するブロック

以下のように記述するととりあえず、テストは行われないが、記述そのものはrspecに認識される
it "test test test"

macherについて

expect(calc.add(2, 3)).to eq(5)         5 と同じか?
expect(calc.add(2, 3)).not_to eq(5)     5 と違うか？
expect(calc.add(2, 3)).to be true       trueか？
expect(calc.add(2, 3)).to be false      falseか?
expect(calc.add(2, 3)).to be > 10       10より多いか？
expect(calc.add(2, 3)).to be_between(1, 10).inclusive   1から10の間か？
expect(calc).to respond_to(:add)        addメソッドはあるか？
expect(calc.add(2, 3)).to be_integer    整数か？


subjectについて
マクロ的なもの？
以下のような書き方が可能
```
RSpec.describe Calc do
    subject(:calc) { Calc.new }     # ここがsubject
    it "given 2 and 3, returns 5" do
        expect(calc.add(2, 3)).to eq(5)
   end
end
```

letについて
変数のようなもの？
```
    context "tax 5% " do
        let(:tax) { 0.05 }      # ここでletを定義
        it { expect(calc.price(100, tax)).to eq(105) }
    end
```

stubについて
あるオブジェクトのメソッドが呼び出された時に特定の値を返却するように設定することができる。
```
t_array = []
test_array.stub(:count).and_return(20)
puts test_array.count   #=>20
```


mockについて
rspecでは以下のようにモックを定義することができる
以下のコードは意味はないが、User#nameを呼び出すと'taguchi'を返却するモックを指定している
```
        user = double('user')
        allow(user).to receive(:name).and_return('taguchi')
        calc = Calc.new
        expect(calc.add(5, 2, user.name)).to eq('7 by taguchi')
```


メソッドが呼び出されたかどうかを判定する
以下のテストケースではlogger#logが呼び出されたかどうかを判定
```
        logger = double('logger')
        expect(logger).to receive(:log) # ここでメソッドの呼び出しを判定
        calc = Calc.new(logger)
        expect(calc.add(5, 2)).to eq(7)
```

テストケース(example)の共有
```
RSpec.shared_examples "basic functions" do
        it "can add"
        it "can subtrace"
        it "can multiply"
        it "can divide"
end

RSpec.describe Calc do
    context "normal mode" do
        include_examples "basic functions"
    end
    context "expert mode" do
        include_examples "basic functions"
    end
end
```


5/18 memo
------------------------

## Rspecの設定
### rspec用ファイルの生成
rails g rspec:install
このコマンドで以下のファイルが生成される

.rspec   rspec用の設定ファイル
spec/spec_helper.rb   ヘルパーファイル
spec/rails_helper.rb  ヘルパーファイル2

### rspecの出力を変更
以下の設定を追加することでrspecの出力を変更することができる
```
/.rspec file
--format documentation
```

### railsのジェネレータにrspecファイルを生成してもらう設定を追加
```
/config/application.rb file

module ContactsRspec3Rails41
  class Application < Rails::Application
    ### add start
    config.generators do |g|
      g.test_framework :rspec,
        fixtures: true,
        view_specs: false,
        helper_specs: false,
        routing_specs: false,
        controller_specs: true,
        request_specs: false
      g.fixture_replacement :factory_girl, dir: "spec/factories"
    end
    ### add end
  end
end
```

### binstubのインストール
以下のコマンドを入力
```
bundle binstubs rspec-core
```

## rspecを実行する#
以下のコマンドを入力し、テストが１件もなければ、"No examples found. "と表示されれば、OK
```
rspec
```


5/19 (Everyday Rails - RspecによるRailsテスト入門 ２章・３章)
------------------------
テストの前にテスト用のDBを作成しないとエラーとなる。
```
root@ab4f339a0c2e:/work/rails-4-1-rspec-3-0# rspec
/usr/local/bundle/gems/activesupport-4.1.1/lib/active_support/values/time_zone.rb:285: warning: circular argument reference - now
/usr/local/bundle/gems/activerecord-4.1.1/lib/active_record/migration.rb:389:in `check_pending!':  (ActiveRecord::PendingMigrationError)

Migrations are pending. To resolve this issue, run:

        bin/rake db:migrate RAILS_ENV=test

        from /usr/local/bundle/gems/activerecord-4.1.1/lib/active_record/migration.rb:395:in `load_schema_if_pending!'
        from /usr/local/bundle/gems/activerecord-4.1.1/lib/active_record/migration.rb:401:in `block in maintain_test_schema!'

```

以下のコマンドでテスト用DBを作成する
```
bundle exec rake db:create RAILS_ENV=test
```

## テストの制御構造
テストは以下のように構造分けすることが可能

```
describe '' do
  describe '' do
    before do # 前処理用のブロック
    end
    context '' do
      it '' do
      end
    end
    context '' do
      it '' do
      end
    end
  end
end
```

5/28(Everyday Rails - RspecによるRailsテスト入門 ４章)
------------------------
## Factory Girlのファイル名と置き場所
書きに示すようにモデル名の複数系。

spec/factories/contacts.rb

## テストデータの記述方法

Factory Girlで使える便利メソッド

sequencesは名前の通り、シーケンシャルな番号を振っていく

記述例
```
FactoryGirl.define do
  factory :contact do     # 形式名(テストコード中のbuildメソッドなどで指定)
    firstname "John"
    lastname "Doe"
    sequence(:email) { |n| "johndoe#{n}@example.com" }
  end
end
```

## テストメソッドからのデータの呼び出し方
以下は'contact'形式のデータを読み込みことを指定。firstnameはnilに上書きしている
```
contact = FactoryGirl.build(:contact, firstname: nil)
```

### FactoryGirlで用意されているメソッド
build   指定データのインスタンスを生成する
create  指定データをDBに作成

### factoryのデータの継承を表現
```
FactoryGirl.define do
  factory :phone do
    association :contact
    phone '123-555-1234'

    # 自宅用
    factory :home_phone do
      phone_type 'home'
    end

    # 仕事用
    factory :work_phone do
      phone_type 'work'
    end

    # 携帯用
    factory :mobile_phone do
      phone_type 'mobile'
    end
  end
end
```
テストプログラムから呼び出す時にはこのようになる
```
    contact = create(:contact)
    create(:home_phone,
           contact: contact,
           phone: '785-555-1234'
          )
```

### factoryのtraitの使い方

account - accounts_plansのように関連がある時、factories内で以下のように表現できる
```
FactoryGirl.define do
  factory :account, class: Account do
    status Account.statuses[:enable]
    postal_code '1080074'
    m_pref_id '13'
    city '港区高輪'
    address_line1  '3-25-29'
    address_line2  'The Site #07'
    organization '株式会社フライデーナイト'
    tel '0364557650'
    billing_reference_date Date.new(2015, 1, 5)

    trait :with_starter_plan do
      after(:create) do |account|
        create(:accounts_plans, account: account, plan_id: 1)
      end
    end

    trait :with_enterprise_plan do
      after(:create) do |account|
        create(:accounts_plans, account: account, plan_id: 4)
      end
    end

  end
end
```
呼び出し側は以下のようにrspec内でデータを作成可能
```
  describe "check_order_array_print_limit?のテスト" do
    let!(:starter_account) {create(:account, :with_starter_plan)}
    let!(:enterprise_account) {create(:account, :with_enterprise_plan)}
```



### Faikerとの連携


tips
========================

POSTでパラメータを渡す方法
------------------------
```
        post :check_coupon, coupon: "12345"
```

リクエストヘッダを変更する方法
------------------------
リクエスト送信前に以下の方法でリクエストヘッダを指定可能
```
        request.env['HTTP_USER_AGENT'] =
          'Mozilla/5.0 (iPhone; CPU iPhone OS 8_1 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko)'
        get :index
```

テンプレートの内容をテストする方法
------------------------
最初にrender_viewsと記述することでviewの記述を行う。
そうするとresponseに反映されるらしい。

```
    context 'mobile device' do
      render_views

      # トップページが表示されること
      it "renders the :index template" do
        get :index
        expect(response.body).to include('あなただけ')  # 描画されたviewの中に’あなただけ'という文言が含まれていること確認
      end
    end
```


マッチャについて
------------------------

be_valid  モデルの妥当性をチェック(バリデーションが全て通る時、true)
eq        イコールの時、true
include   含まれている時、true
