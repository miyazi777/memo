
## redirect_to

redirect_to 遷移先URL, パラメータ

```
redirect_to action: "show", id: 5
redirect_to post
redirect_to "http://www.rubyonrails.org"
redirect_to "/images/screenshot.jpg"
redirect_to articles_url
redirect_to proc { edit_post_url(@post) }
```

## rendar

rendar 遷移先, パラメータ

```
render action: "show"
```


前処理・後処理
----------------------------------------

before_action <method>  アクションの前に処理を行うことができる
after_action <method>   アクションの後に処理を行うことができる
around_action <method>  アクションの前後に処理を行うことができる

#### 実装例
以下の処理ではアクションが実行される前にsetupメソッドの処理が実行される
```
class DatasController < ApplicationController
    before_action :setup

    private
        def setup
            なんらかの処理
        end
```

#### 実行条件をつける
メソッドの名の後に実行条件を付加することもできる

:only   指定されたアクションの時のみ、前後処理が実行される
:if     指定された条件に合致した時のみ、前後処理が実行される

下の例ではshow、editアクションが呼び出される時のみ、setupが前処理として実行される
```
before_action :setup, only:[:show, :edit]
```




パラメータの受け取り方
----------------------------------------

/users/:idのようなURLでアクセスした場合、controllerでURLのパラメータはparamsに格納されている。
以下のようなコードで取得できる。

```
id = params[:id]
```

#### paramsのメソッドについて
require 対象のモデル名を指定
permit  取得する項目名を制限

##### 実装例
```
      params.require(:user).permit(:name, :email)
```

