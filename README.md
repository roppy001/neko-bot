# maho-bot
プリコネR クランバトル用bot

# コマンド
修正案
~~~
■予約コマンド
.re ボス番号[@周 or +] 予定ダメージ(万単位)[m[何凸目か]] [コメント]
+が書かれた場合は次の周の予約
周が省略された場合は現在の周、

＜利用例＞
.re 5 3700 ワンパン予定 ⇒ 現在の周の5ボスの予約
.re 3+ 700m ⇒ 次の周の持ち越し予約
.re 3@23 700m1 ⇒ 23周の3ボスの1凸目の持ち越し予約


■凸完了コマンド
.fin ボス番号 実績ダメージ(万単位)[m[何凸目か]]

＜利用例＞
.fin 5 2500 ⇒ 5ボスに本凸 2500万ダメージ
.fin 3 700m ⇒ 3ボスに持ち越し 700万ダメージ
.fin 2 700m1 ⇒ 2ボスに1凸目持ち越し 700万ダメージ


■討伐コマンド
.la ボス番号 [持ち越し秒数 or m[何凸目か]]

＜利用例＞
.la 5 90 ⇒ 5ボスに本凸 90秒残し
.la 5 m ⇒ 5ボスに持ち越し
.la 5 m3 ⇒ 5ボスに3凸目持ち越し


■取り消しコマンド
.cl ⇒ 自分の予約をすべて削除

■ステータス変更コマンド
.ms ステータス
.ms 000 ⇒ すべて未凸状態にしたあと、予約をすべて削除する
.ms 120 ⇒ 1凸目を持越、2凸目を凸済、3凸目を未凸状態にした後、予約をすべて削除する

~~~

## 管理用コマンド
~~~
■クラバト参加コマンド
.add [メンション(複数指定可)]

＜利用例＞
.add ⇒ 自分が参加
.add @あああ @いいい ⇒ あああといいいが参加

■クラバト脱退コマンド
.remove [メンション(複数指定可)]

＜利用例＞
.remove ⇒ 自分が脱退
.remove @あああ @いいい ⇒ あああといいいが脱退

■bot鯖脱退コマンド
.kickbot ⇒ botがdiscord鯖から抜ける

■ボス状態変更コマンド
.ms ボス番号 周 [HP]

.ms 1 1 ⇒ 1ボスの周を1に変更して、HPをMAXにする
.ms 1 3 300⇒ 1ボスの周を13に変更して、残りHPを300万にする



~~~



# 導入方法(仮)
※不要な手順もあるかもしれないが、自分は下記でうまく行ったのでひとまず掲載

1. python 3.8 以降の最新版をインストール
2. 以下のコマンドを打つ
~~~
py -3 -m pip install -U discord

python3 -m venv env

env\Scripts\activate.bat
pip install -U discord
~~~
3. run.batを実行


動作環境
python 3.8.10
windows 10

# コマンド詳細

## 全コマンド共通
 * []で囲まれたものは省略可能
 * 末尾にメンションを付けると代行入力が可能。複数のメンションが付けられた場合は入力エラーとなる。
 * コマンドの間は1個以上の全角スペースもしくは半角スペースで空ける
 * 数字に数字以外のもの、入力できる範囲外の数字を入力した場合は入力エラーとなる
 * 全角数字は許容する
 * コマンドの英字大文字指定は許容する
 * 5時をまたいだ際、前日の予約・実績情報は全て削除される

## クラメン用標準コマンド

### 予約コマンド
#### コマンドの使い方
~~~
.reserve ボス番号[@周 or +] 予定ダメージ(万単位)[m[何凸目か]] [コメント]

エイリアス
.re 
.予約 
~~~

#### コマンド打った時の動き
 * コマンドで指定された周、ボスに予約を入れる。予約リストに表示がされるようになる
 * 凸の予約が入る。
   * 基本的に追加の予約として登録される。既存の予約の変更はできない。
   * 凸指定なし予約の場合、ユーザーステータス、予約済状況を踏まえ、最も若い凸の予約が入る。予約ができない場合はエラーとなる
   * 本凸予約が持ち越し予約に置き換わることはない。
 * 周が0の場合は、周未指定の予約として予約リストに表示がされる
 * コマンド実行時に、予約登録時刻が記録される。予約リストは、予約登録時刻順に表示がされる。
 * すでに討伐済みの周の予約がされた場合は、エラーとなる

### 凸完了コマンド

#### コマンドの使い方
~~~
.finish ボス番号 実績ダメージ(万単位)[m[何凸目か]]

エイリアス
.fin
.完了
~~~

#### コマンド打った時の動き
 * コマンドで指定された周、ボスの実績登録がされる。予約リストに実績として表示がされるようになる。
 * 指定された凸の実績登録が行われる。実績登録がされるとともに、予約が以下のルールに従って削除される。
   * 1凸目の実績登録をした際に、同じボスに複数の予約登録があった場合、最も凸数目の小さい予約が削除される
   * 1凸目の実績登録をした際に、1凸目の予約、1凸目の持ち越し予約が別のボスに入っていた場合、1凸目の予約、1凸目の持ち越し予約が削除される
   * ユーザステータスが1凸目持越の時に1凸目の実績登録をした場合、持ち越しの実績登録扱いとなる。
   * ユーザステータスが1凸目完了の時に1凸目の実績登録をした場合、入力エラーとなる。
   * 上記は2凸目以降も同等となる
 * 指定の凸のユーザステータスを凸完了済に変更する
 * コマンドに指定されたダメージをもとに、敵の残り実績HPが計算され、予約リストに表示される
 * コマンド実行時に、予約がされてなかった場合は登録時刻が記録される。予約リスト内の実績リストには、登録時刻順で表示がされる。上書きの場合は登録時刻は更新されない。
 * すでに討伐済みの周の実績登録がされた場合は、エラーとなる

### 討伐登録コマンド

#### コマンドの使い方
~~~
.lastattack ボス番号 [持ち越し秒数 or m[何凸目か]]

エイリアス
.la
.討伐
~~~

#### コマンド打った時の動き
 * コマンドで指定された周、ボスの討伐実績登録がされる。予約リストに実績として表示がされるようになる。
 * 指定された凸の討伐実績登録が行われる。討伐実績登録がされるとともに、予約が以下のルールに従って削除される。
   * 1凸目の討伐実績登録をした際に、同じ周の同じボスの予約は全て削除される。
   * 1凸目の討伐実績登録をした際に、1凸目の持ち越しではない予約が別のボスに入っていた場合、1凸目の持ち越しではない予約が削除される
   * ユーザステータスが1凸目持越の時に1凸目の討伐実績登録をした場合、持ち越しの討伐実績登録扱いとなる。
   * ユーザステータスが1凸目完了の時に1凸目の討伐実績登録をした場合、入力エラーとなる。
   * 上記は2凸目以降も同等となる
 * 指定のボスの周状態が次の周に変更される
 * 指定の凸のユーザステータスが持ち越しの場合、ユーザステータスを凸完了に変更する
 * 指定の凸のユーザステータスが未凸の場合、ユーザステータスを持ち越しに変更する
 * コマンドに指定されたダメージをもとに、敵の残り実績HPが計算され、予約リストに表示される
 * コマンド実行時に、予約がされてなかった場合は登録時刻が記録される。予約リスト内の実績リストには、登録時刻順で表示がされる。上書きの場合は登録時刻は更新されない。
 * すでに討伐済みの周の実績登録がされた場合は、エラーとなる

### キャンセルコマンド

#### コマンドの使い方
~~~
.cancel ボス番号[@周 or +] [何凸目か or m[何凸目か]]

エイリアス
.cl
.取消
~~~

#### コマンド打った時の動き
 * 指定の周のボス番号の予約もしくは実績が削除される。

### ステータス変更コマンド

#### コマンドの使い方
~~~
.modifystatus ステータス
ステータス：000 などの3桁の数字で表される 0:未凸 1:持ち越し 2:凸完了 を表す

エイリアス
.ms
.状態変更
~~~

#### コマンド打った時の動き
 * ユーザステータスが指定のステータスに変更される。
   * 左から1凸目、2凸目、3凸目を表す

## 管理用コマンド

### ボス周変更コマンド
#### コマンドの使い方
~~~
.modifyboss 周 ボス番号
~~~

#### コマンド打った時の動き
 * 指定のボスが指定の周に変更される。

### ボス予約実績取り消しコマンド
#### コマンドの使い方
~~~
.cancelboss 周 ボス番号
~~~

#### コマンド打った時の動き
 * 指定のボスが指定の周の予約、実績がすべて削除される

#### コマンドの使い方

### 全予約実績取り消しコマンド
#### コマンドの使い方
~~~
.cancelall 周
~~~

#### コマンド打った時の動き
 * 指定の周および指定の周以降の予約・実績がすべて削除される
 * 周が0の場合、全ての予約・実績がすべて削除される

