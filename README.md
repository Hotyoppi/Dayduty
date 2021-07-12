# 日直メールの設定方法

## 宮地研メールサーバーへのログイン

```
$ ssh today@crypto-cybersec.comm.eng.osaka-u.ac.jp
$ ssh today@133.1.144.136
```
上記のどちらでもログイン可能。pwはcy2secNow

## 日直メールの仕組み
ログインした後、直下に次のようなファイルがあるはず
```
list.txt
mail.txt
schedule.txt
MemberList.txt
line_notify.py
mailtest.sh
```
- メール送信までの流れ
  - crontabによって、mailtest.shが実行(毎朝7時)
  - list.txtに該当する日時があれば参照して，mail.txt(メール本文)を構築
  - schedule.txtから該当する日の情報があれば，２日後まで参照しmail.txtに追加
  - メーリングリストのlab-duty@crypto-cybersec.comm.eng.osaka-u.ac.jp宛に本文(mail.txt)を送信

- ライン通知までの流れ
  - crontabによって、[line_notify.py](https://notify-bot.line.me/doc/ja/)が実行(毎朝10時)
  - 該当日の7時にmail.txtが更新されていれば、その日の日直を参照してラインのメッセージとして通知

## 日直メールのメンバー更新

実際にメンバーを管理しているのは、
```
/home/today/crontab_labduty/main.py
```
であり、MemberList.txtに日直を回したいメンバーを一人ずつ改行して記述することによって、
main.pyが平日の日程とメンバーを関連付けてlist.txtを出力してくれる。
ただ、このmain.pyがどのように実行されているのかは現在不明
(mailtest.shを起動するとなぜか、list.txtも更新されるが、mailtest.shにはmain.pyを実行する記述はない Orz)

##　やればいいこと(ここだけ読んでくれたらOK)

- 日直のメンバーを更新したい
  - Memberlist.txtを更新すれば良い
  - 現在のlist.txtの情報とMemberlist.txtの情報を参考に更新されるので、Memberlist.txtの順番に注意(必ず１行目から始まるわけではない)

- 日直メールの予定を更新したい
  - 秘書さんから送られてくるメールをschedule.txtに更新
  - vimで行う場合(手動)　ノーマルモードでggVGdと入力し元の全文を削除し、入力モードにしたのちメールをペースト。
  - 行頭が06/01のようになっていないとしっかり参照されないので、注意(6/01では認識しない)


