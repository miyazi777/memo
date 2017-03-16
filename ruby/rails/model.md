
# active recordでモデルのインスタンスの特定の値で更新する

取得レコードをハッシュで指定した値で更新する

```
entity = User.find(1)
entity.attributes = {name: "updatename", age: 55}
if entity.invalid?
  # error process...
end
entity.update
```

# enumの値を指定して更新する
以下のコードではenumで定義したtype2にhogeモデルが更新される。

```hoge.rb
  enum type: {type1:0, type2:1, type3:2}
```

```controller.rb
  hoge = Hoge.new
  hoge.type2!   <= type2に更新するアップデート文が実行されるので注意。
```

アップデートしたくない場合は以下ように値を設定する
```controller.rb
  hoge = Hoge.new
  hoge.type = Hoge.types[:type2]
```


# validationのエラーメッセージを変更する
----------------------------------------
下のコードはfeild_typeにenumで指定した以外の値が指定された時に以下のようなエラーメッセージが出力される。

出力されるエラーメッセージ：フィールドタイプが正しくありません。

```template_custom_field.rb
  enum field_type: {type_text: 0, type_text_alphanumeric: 1, type_text_numeric:2, type_text_double:3, type_image:4}

  validates :field_type, inclusion: {in: TemplateCustomField.field_types.keys, message: "が正しくありません。"}
```

```config/locales/ja.yml
  activerecord:
    attributes:
      template_custom_field:
        field_type: フィールドタイプ
```


# orをどうするか？
----------------------------------------
以下のgemを使ってみる
https://github.com/Eric-Guo/where-or

以下のようなコードでorを発行できるようになる
```
User.where('id = ?', id).or(User.where('name like ?', "%hoge%"))
```

