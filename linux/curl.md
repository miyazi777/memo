概要
===============================

コマンドラインからHTTPリクエストを送信できるコマンド

-H  ヘッダーを指定
-X  メソッドを指定
-F  フォームデータを指定

## 基本系

```
curl http://yahoo.co.jp
```

## 応用例

```
curl -H "Content-type: multipart/form-data" -H "Authorization:Basic NTVmMGI4MjQ0NTJmNWJmNjg5OGE6NjAwMzMwYWQxZjcwNTg2YmJhNjJkM2M2\nMjVhOWZmOGI=\n" -X POST -F 'confirmation=false' -F 'address_line1=荒町1-14-5' -F 'address_line2=自宅' -F 'city=三条市' -F 'custom_fields=[{"id":1, "value":"#image1"}]' -F '#image1=@test.png' -F 'name=test_order' -F 'name_furi=テスト2' -F 'organization=株式会社テスト2' -F 'postal_code=1450023' -F 'pref=新潟県' -F 'tel=0256333597' -F 'template_id=1' http://localhost:3000/api/v1/orders/simple
```
