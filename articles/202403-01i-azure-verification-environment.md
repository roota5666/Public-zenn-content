---
title: "Azure検証環境の管理手法案"
emoji: "⚙️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [ "azure","アカウント管理"]
published: false
publication_name: "ap_com"

date: 2024-03-28
url: "https://zenn.dev/roota5666/articles/202403-01i-azure-verification-environment"
tags: ["#Zenn #2024-03"]
aliases: 記事「Azure検証環境の管理手法案」
---

## はじめに

本記事では、Azure検証環境の管理手法についての自分なりに考えたことを記載しようと思います。

### やりたいこと

検証環境の「誰が作ったresourceGroupなの！？」問題へ終止符を打つ！

## 環境

- Azure *1

*1 ユーザは自由にリソースグループを作成できる権限を持っている想定です

## 本題

### こんなことありませんか？

とある検証環境のリソースグループの一覧

- rota-test
- sato-poc
- udemy_rg
- sendgrid_rg
- domain
- poc_test

はい！  
どれが今使われているの！？誰が管理者！？

ってなりますね（ ＾ω＾）

### 管理手法案

「resourceGroupにownerタグを付ける」

これだけです。

タグとは？

>タグは、Azure リソースに適用するメタデータ要素です。 これらは、組織に関連する設定に基づいてリソースを識別するのに役立つキーと値のペアです。
引用元 <https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/management/tag-resources>

今回はownerというタグにUIDを記載します。

| 名前  | 値      |
| :---- | :------ |
| owner | 【UID】 |

UIDはメールアドレスや社員番号などの重複のない番号です。

>システムやサービスに利用者登録（アカウント作成）を行う際に設定する利用者名（ユーザー名/アカウント名）のことをUIDということがある。
引用元 <https://e-words.jp/w/UID.html>

### タグ付け

やってみましょう。  
難しいことはありません。

各リソースの左メニューに**タグ**があるはずです。

リソースグループの例です。タグをクリック
![](/images/202404-01t-azure-verification-environment/202404-01t-azure-verification-environment_001.png)
名前と値を入力します
![](/images/202404-01t-azure-verification-environment/202404-01t-azure-verification-environment_002.png)
タグ付けされました！
![](/images/202404-01t-azure-verification-environment/202404-01t-azure-verification-environment_003.png)

### 設定している人のリスト取得

以下は管理者視点の操作です。Azure Cloud Shellを使ってownerタグが設定されているリソースグループの一覧を取得します。

```bash
az group list --query "[?tags.owner != null ].{name:name,owner:tags.owner}" \
              --output table
```

結果出力例

```bash
r_ota [ ~ ]$ az group list --query "[?tags.owner != null ].{name:name,owner:tags.owner}" --output table
Name        Owner
----------  ---------------------
sato        sato@example.com
suzuki      suzuki@example.com
takahashi   takahashi@example.com
tanaka      tanaka@example.com
ito         ito@example.com
watanabe    watanabe@example.com
yamamoto    yamamoto@example.com
nakamura    nakamura@example.com
kobayashi   kobayashi@example.com
r_ota [ ~ ]$ 
```

:::details テスト用データ - リソースグループを10コ作る/消す

:::message
実行前に同じ名前のリソースグループが既に存在していないか注意してください
:::

- テスト用リソースグループ作成
  
  ```bash
  az group create --location japaneast --resource-group sato      --tags owner=sato@example.com
  az group create --location japaneast --resource-group suzuki    --tags owner=suzuki@example.com
  az group create --location japaneast --resource-group takahashi --tags owner=takahashi@example.com
  az group create --location japaneast --resource-group tanaka    --tags owner=tanaka@example.com
  az group create --location japaneast --resource-group ito       --tags owner=ito@example.com
  az group create --location japaneast --resource-group watanabe  --tags owner=watanabe@example.com
  az group create --location japaneast --resource-group yamamoto  --tags owner=yamamoto@example.com
  az group create --location japaneast --resource-group nakamura  --tags owner=nakamura@example.com
  az group create --location japaneast --resource-group kobayashi --tags owner=kobayashi@example.com
  ```

- テスト用リソースグループ削除
  
  ```bash
  az group delete --yes --name sato
  az group delete --yes --name suzuki
  az group delete --yes --name takahashi
  az group delete --yes --name tanaka
  az group delete --yes --name ito
  az group delete --yes --name watanabe
  az group delete --yes --name yamamoto
  az group delete --yes --name nakamura
  az group delete --yes --name kobayashi
  ```

:::

### 設定していない人のリスト

設定している人のリスト取得ができたので、タグが設定されていないリソースグループ一覧を取得します。

```bash
az group list --query "[?tags.owner == null ].name" \
              --output table | sort
```

除外ver. cloud-shell-storage-*

```bash
az group list --query "[?tags.owner == null && name != 'cloud-shell-storage*' ].name" \
              --output table | sort
```

ownerタグが設定されていないリソースグループについては、所有者を確認して削除やタグ付与の対応をしましょう。

## 感想&まとめ

検証環境あるあるだと思いますが「だれが何の目的で作ったかわからないリソースがある」を少しでもなくせるように考えてみました。  
自分でも「test-cent7」や「zabbix-test」など、後で見て何に使ったのかわからないリソースを見て、イラッとしたことがありました。。  
ローカルのリソースなら電気代だけで済みますが、クラウドはたちあげっぱなし＝お金がチャリンチャリン使われていくことになります。  
少しでも節約/効率的に使いたいですよね。  
また、会社で使える検証環境をゴミだらけにしたくない！です。  

少しでも役に立てばうれしいです。  
最後までお読みいただきありがとうございました。

## 参考サイト

- @[card]()
- @[card]()
- @[card]()
