## 通常の rsync 同期で、オーナやパーミッションを保持して転送する場合。

## 同期用ユーザを作成する
前提として
+ syncuser はパスワード無しで sudo 可能
+ syncuser はパスフレーズなしの鍵認証でログイン可能
+ syncuser にはパスワードが設定されていない

よりセキュリティを高めるには、`/etc/ssh/sshd_config` に `AllowUsers` で `syncuser@10.0.0.4` のみに制限するなど。
その場合、全ユーザに対して制限がかかるので、以下のようにログインする必要があるユーザ名を羅列する。

    AllowUsers  ec2-user
    AllowUsers  syncuser@10.0.0.4

## lsyncd をインストールする
## lsyncd を設定する
## lsyncd の動作テストをする
## lsyncd を起動する
