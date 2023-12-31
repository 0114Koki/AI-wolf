# 本サービスで使うDynamoDBのTableについて
参考: https://www.wakuwakubank.com/posts/639-aws-dynamodb-introduction/

## game_manager(ゲーム管理テーブル)
1ゲームを1行で管理するテーブル。ゲームの進行状態やメンバー識別子などを記録する。\
- GameID: 1から順に振っていく整数。
- RoomID: ランダムな6桁の整数。
- n_mem: そのゲームに参加する人数(3~8人)
- OwnerID: 部屋の作成者の識別子
- UserIDs: 参加者の識別子(リスト)
- Dead: 死亡者のユーザー名のリスト(["ゆい", "はる"])
- AI_messages: {"ゆい": [ChatGPTに送るmsg(system,assistant,user)], "みく": []} # それぞれのHackedを担当するAIのmsgを格納
- GameState: ゲームの状態
    - 0: 募集中
    - 1: ゲーム中
    - 2: ゲーム終了
    - 3: ゲーム取り消し・中断
- CreatedAt: ゲームが作成された時刻(yyyymmdd-hh-mm-ss形式で文字列)

|Partiation Key|SortKey|Attribute|Attribute|Attribute|Attribute|Attribute|Attribute|
|--|--|--|--|--|--|--|--|
|GameID|RoomID|n_mem(人数)|OwnerID|MemberIDs|n_hacked|Hacked|GameState|created-at|
|1|703712|5|"ajlra321"|["daow121", "daw131", "sgsrg21", "fjea412"]|1|[]|0|yyyymmdd-hh-mm-ss|
|2|732712|4|"daow134"|["ddawow121", "daw131", "fjea412"]|1|[]|0|yyyymmdd-hh-mm-ss|


## ゲーム進行テーブル
ゲーム内での発言などを記録するテーブル。
|質問ID|ゲームID|回答||

## 質問テーブル
事前に用意した質問から出題するモードで使う質問を格納しておくテーブル

# 参考にしたもの
 - 「API Gateway + LambdaでREST API開発を体験しよう [10分で完成編]」
 https://qiita.com/tamura_CD/items/46ba8a2f3bfd5484843f
 - 今回は、GETメソッドのAPIを作成します。
     - 1: Lambdaで、APIの内部処理を担当する関数を作成する。
     - 2: API Gatewayで、REST APIを作成して、Lambda関数を繋げて、APIを完成させる。
