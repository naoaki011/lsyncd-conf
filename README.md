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
yum の epel リポジトリを有効にする
- RHEL7

    ```sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm```

- RHEL8ではcodeready-builderリポジトリを有効にする必要がある

    ```sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm```
    ```sudo dnf config-manager --set-enabled codeready-builder-for-rhel-8-rhui-rpms```

- AWSのAmazon Linux 2ではEPELを有効にする専用のコマンドがある

    ```sudo amazon-linux-extras install -y epel```

- CentOS 7 では、ベースリポジトリに epel-release パッケージが含まれています。

    ```sudo yum install -y epel-release```

- CentOS 8 では、RHEL 8 用の EPEL リリースパッケージをインストールします。EPEL リポジトリと PowerTools リポジトリの両方を有効にします。PowerTools リポジトリには、多くの EPEL パッケージに必要な開発ツールが含まれています。

    ```sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm```
    ```sudo dnf config-manager --set-enabled PowerTools```

lsyncd をインストールする

    sudo yum install -y --enablerepo=epel lsyncd

## lsyncd を設定する
/etc/lsyncd.conf が設定ファイルになるので、適宜書き換える。
サンプルとして、添付。

## lsyncd の動作テストをする
上記設定ファイルを使い、起動してみる

動作状況やエラー発生は、ログを確認しつつテストする


## lsyncd を起動する
上記で問題なく動作するようならば、登録済みのサービスを起動する
サービスの自動起動を有効にする
