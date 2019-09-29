# 仕様のメモと提出するやつ

## usersテーブル
|Column|Type|Options|
|------|----|-------|
|user|text|null: false|
|email|text|null: false, unique|
|password|text|null: false|

### Association
- hasmany_to :groups
- hasmany_to :messages
- hasmany_to :imgs

## groupsテーブル
|Column|Type|Options|
|------|----|-------|
|name|text|null: false|
- hasmany_to :messages
- hasmany_to :imgs

## group_mastarsテーブル(グループ作成者のフラグ管理)
|Column|Type|Options|
|------|----|-------|
|user_id|integer|null: false, foreign_key: true|
|group_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :group
- belongs_to :user

## messagesテーブル
|Column|Type|Options|
|------|----|-------|
|text|string|null: false|
|user_id|integer|null: false, foreign_key: true|
|group_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :group
- belongs_to :user

## imgsテーブル
|Column|Type|Options|
|------|----|-------|
|img|string|null: false, unique|
|user_id|integer|null: false, foreign_key: true|
|group_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :group
- belongs_to :user

## user_messegeテーブル(インデックス)
|Column|Type|Options|
|------|----|-------|
|user_name|integer|null: false, foreign_key: true|
|group_name|integer|null: false, foreign_key: true|
|message_text|integer|null: false, foreign_key: true|

### Association
- belongs_to :group
- belongs_to :user



# 機能洗い出しメモ
## ログイン
- user承認機能の実装
- 「アカウント登録もしくはログインしてください。」オレンジ帯表示
- 登録していたら項目入力でlogin
- Sign upでCreate Accountに遷移

## ログアウト
- topに戻るリダイレクト作成

## アカウント
- 登録
- userテーブル作成
- 「アカウント登録が完了しました。」水色帯表示
- デフォルトはグループ無し

## 編集
- userテーブル編集
- 名前とメールアドレスを編集
- ログアウトボタン、TOPページに戻る

## グループ
### -作成
- チャットメンバー追加
- グループ名入力
- プレースホルダには「グループ名を入力してください」「追加したいユーザー名を入力してください」
- 登録するボタンを押すと登録され、TOPにリダイレクト「グループ作成しました」の表示水色帯
- 登録の際の必須項目はグループ名のみ

#### --チャットメンバー検索
- JSのリアルタイム検索のアレで探す
- チャットメンバー追加（表示上は追加されているが保存するまで物理追加はされてない）
- 追加されたら検索の一覧から消える
- チャットメンバー削除→実際には保存するまで追加はされてないので、表示上なくなる
- グループマスター（作成者は削除不可）←公開されている仕様にはないが対応できるように考えておく　実機ではカレントユーザー自体は削除できない仕様

#### --サイドバーでの表示
- 作成されたグループは所属グループの一番下に新しく作成したグループが表示される
- 作成後はTOPにリダイレクト（この時作ったチャットグループの表示はしない）←したほうが便利そうではある
- 登録されるとサイドバーにグループ名増えてる（リアルタイムではない）、更新すると表示される

### -編集
- グループ名入力（現在のグループ名が入った状態）
- 【作成と同じ】プレースホルダには「グループ名を入力してください」「追加したいユーザー名を入力してください」
- 登録するボタンを押すと登録され、編集した該当グループに遷移「グループを編集しました」の表示水色帯
- 【作成と同じ】登録の際の必須項目はグループ名のみ（チャットメンバーを全て消しても保存は通る）

#### --チャットメンバー検索
- 【作成と同じ】JSのリアルタイム検索のアレで探す
- 【作成と同じ】チャットメンバー追加（表示上は追加されているが保存するまで物理追加はされてない）
- 【作成と同じ】チャットメンバーに追加されたら検索の一覧から消える
- 【作成と同じ】チャットメンバー削除　→　実際には保存するまで追加はされてないので、表示上なくなる
- 【作成と同じ】グループマスター（作成者は削除不可）←公開されている仕様にはないが対応できるように考えておく　実機ではカレントユーザー自体は削除できない仕様

#### --サイドバーでの表示
- 編集後はTOPに編集したチャットルームに戻る
- 編集されるとサイドバーにグループ名が更新される（リアルタイムではない）、更新すると表示される


## チャット
- メッセージを書き込む
- メッセージがチャット画面に表示される（リアルタイム）
- サイドバーの最新のチャット1件が表示更新される（リアルタイムでない）
- URLはクリッカブルにならない←なったほうが便利なのでメモ
- 画像の送信が遅かったらテキストなどが優先される（早く送れるほうを優先する）
- 画像が送られたら最新コメントに「画像が投稿されました」←ここは分岐でdb使わない
- 画像投稿コメントとは紐づかない
- お話中のキックされてもそのチャットルームに残る、更新されるといなくなる（グループ内のユーザー管理はリアルタイムでない）
- キックしてもチャット履歴は残る（ユーザーがいなくなっても、投稿されたコメントはなくならない）
- キックされた人の履歴の名前も更新される(つまりグループから抜けてもメッセージとコメントは紐づいてる)


