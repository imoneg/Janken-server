# Janken-server
学校の課題でじゃんけんをしろと言われたので

# じゃんけんサーバー仕様  

## API一覧

|No|API名|機能概要|
|--|--|--|
|0|じゃんけんを出す|じゃんけんをする|
|1|結果表示|じゃんけんの試合結果を表示する|
|2|一覧表示|じゃんけんの試合結果の一覧を表示する|

## API詳細  

### No 0 じゃんけんを出す

* 入力
	* GET url/vote  
	
| GETパラメータ | 型 | 必須? | 値の範囲 | 説明 |
|:--|:--|:--|:--|:--|
|id|int|〇|0~INT_MAX|事前に話し合いで決めるクライアントのid|
|selection|int|〇|0~2|0=>グー,1=>チョキ,2=>パー じゃんけんの手|
|battle_id|int|〇|0~INT_MAX|何試合目かを示すid、インクリメントされていく|


出力  
JSON形式  

| JSON Key | 型 | 値の説明 |
|:---|:---|:---|
|result|int|下の表を参照|
|message|文字列|resultの説明文.日本語が入るかもしれない|

resultの表  
0が正常,0以外はエラーです  

| resultの値 | 内容 | 説明 |
|:---|:---|:---|
|0|正常に完了|じゃんけんの手がアクセプトされたときの応答|
|1|すでに投票済み|すでにじゃんけんの手を出しているのにもう一度出そうとしたとき|
|2|試合が不連続|battle_idが完了済みの試合+1よりも大きい場合|
|3|パラメータ異常|getのパラメータが足りなかったり,値がおかしい場合
|4|不明なエラー|なんかおかしいんじゃないですかね|

例  
* 入力  
url/vote?id=0&selection=1&battle_id=2
じゃんけんクライアントのidは0で,出す手はチョキ(1),試合は2試合(0試合目があるので合計3試合)

* 出力
{"result": 0, "message" : "accepted"}
正常にじゃんけんの手を出せた

### No 1 結果表示

* 入力  
	* GET url/result  

| GETパラメータ | 型 | 必須? | 値の範囲 | 説明 |
|--|--|--|--|--|
|id|int|〇|0~INT_MAX|事前に話し合いで決めるクライアントのid|
|battle_id|int|〇|0~INT_MAX|何試合目かを示すid  

 
|JSON Key| 型 |値の説明|
|--|--|--|
|result|int|下の表を参照|
|message|文字列|resultの説明文.日本語が入るかもしれない|
|victory| "win" or "lose" or "draw" 文字列|勝ちか負けかあいこか|
|  game|オブジェクト|下の要素が中身になる|
|id|文字列,int|keyがクライアントのid(文字列になっている)で,valueはそのクライアントの出した手(0=>グー,1=>チョキ,2=>パー)(こっちはint)|

resultの表  
0が正常,0以外はエラーです    

| resultの値 | 内容 | 説明 |
|--|--|--|
|0|正常に完了|じゃんけんができたとき|
|1|未決着|全員分のじゃんけんの手がまだそろっていない場合|
|2|未定義|特に今のところ使っていない|
|3|パラメータ異常|getのパラメータが足りなかったり,値がおかしい場合
|4|不明なエラー|なんかおかしいんじゃないですかね|


* 入力の例
url/result?id=0&battle_id=2
クライアントidが0で,2試合(0試合目から始まるので3試合だけど)の結果

* 出力の例1
{"result":0,"message":"janken","victory":"draw", "game":{"0":0,"1":2,"2":0,"3":1}}
試合が正常に行われて、結果は引き分けだった
それぞれの出した手は
id 0 => 0(グー)
id 1 => 2(パー)
id 2 => 0(グー)
id 3 => 1(チョキ)

* 出力の例2
{"result":1,"message":"まだだよ"}
じゃんけんの手が出そろっていないとき

### No 2 一覧表示

未実装
多分実装されることはない
