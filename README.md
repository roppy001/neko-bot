# neko-bot
プリコネR クランバトル用bot

## このBOTでできるようになること
### 実現優先度高め
 * クランメンバの凸状況が把握できるようになる
 * クランメンバがどの周のどのボスに凸をしたいかが把握できるようになる
 * 現在の各予約でのダメージが把握でき、ボスの討伐に人数が足りてるかどうかが把握できるようになる
 * 7月以降のクラバトの仕様に対応する
   * 全ボスの残りダメージが管理できること
   * メンバのステータスは1凸目(未／持越／済)、2凸目(未／持越／済)、3凸目(未／持越／済)の組み合わせで表現する
   * 各予約が、どの凸のどの本凸／持越なのかを管理できる
   * これまでは「どの周の何ボス目か」が進行状況を表していたが、今後は「どの周か」のみになる
 * 全ボス撃破したときに、次の周の予約をしている人に対してメンションでお知らせしてくれる
 * クランメンバ合計の凸状況が把握できるようになる
 * 誤ったコマンドを打った時に、別のメンバで修正ができる
### 実現優先度低め
 * クランメンバ合計の凸状況と5時までの時間を計測し、今のペースが速いのか遅いのかが把握できるようになる
 * 凸宣言後40分経過するとメンションで知らせてくれる
### 実現予定なし
 * 凸中かどうかのステータス管理
   * 凸コマンドは不要
 * 同時凸／ダメコン時のダメージ状況管理
   * チャットで直接やり取りすればOKなので不要

## コマンド









