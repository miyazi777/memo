generateコマンド
===============================

### モデル追加
```
rails generate model <model-name>
```

migratoinファイルの例
```
create_table :factory_user_roles do |t|
  t.references :factory_user, index: true, foreign_key: true, null: false
  t.integer :role, null: false
  t.boolean :write_auth, null: false
  t.timestamps null: false
end
```

### カラム追加
```
rails g migration add_process_status_to_order process_status:integer

#### ダメかもしれないやり方
rails generate migration AddColumnToUser piyo:string puyo:integer
  このやり方は同じmigrationファイル名のファイルがある時にコンフリクトが起きて使えない

```

migrationファイルの例
```
  def change
    add_column :media, :original_filename, :string, null: true, after: :file_tmp
  end
```

migrationファイルの例2
```
  def change
    add_reference :user_informations, :account, index: true, foreign_key: true, null: true, after: :id
    add_column :user_informations, :broadcast_flg, :boolean, default: false, null: false, after: :account_id
  end
```

### カラム名変更
```
rename_column :articles, :title, :changed_title
               <table_name> <bedore_name> <after_name>
```

### カラム削除
rails g migration remove_account_id_to_pending_config account_id:integer


### ユニークインデックス追加
rails g migration add_index_to_order

#### migrationファイル例

```
class AddIndexToOrder < ActiveRecord::Migration
  def change
    add_index :orders, :uid, :unique => true
  end
end
```

### モデル削除
以下のコマンドでモデルとテストケースなどは削除されるが、DB上のテーブルは消えない
```
rails destroy model <model-name>
rails d model <model-name>
```

### テーブル削除
以下のコマンドでmigrationファイルを作成後、削除用のスクリプトを記述し、最後、rake db:migrateすることでテーブルを削除

```
rails g migration drop_table_prepress_records
```

drop_table_prepress_records.rb
```
class DropTablePrepressRecords < ActiveRecord::Migration
  def change
    drop_table :prepress_records
  end
end
```





migrationファイル内の記述
===============================

t.<data-type> :<item-name>, 各項目ごとの指定1, 各項目ごとの指定2


### 指定できるデータタイプ
string: 文字列
text: 長い文字列
integer: 整数
float: 浮動小数
decimal: 精度の高い少数
datetime: 日時
time: 時間
date: 日付
boolean: Bool値
timestamps: タイムスタンプ(created_at,updated_at)
references: 外部キー。項目名にはテーブル名を指定すること

### null指定
null: false     not null指定

### index
index: true     インデックス指定

### default
default: "defaultの値"

### 外部キー
foreign_key: true   外部キー

### ユニークインデックス
unique: true


### 外部キーの削除
```
remove_foreign_key :pending_configs, column: :account_id
```

