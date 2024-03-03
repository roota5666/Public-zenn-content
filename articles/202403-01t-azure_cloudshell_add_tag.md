---
title: "Azure Cloud Shellで使用するストレージにタグを追加する"
emoji: "🎈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [azure,cloudshell]
published: true
publication_name: "ap_com"

date: 2024-03-03
url: "https://zenn.dev/roota5666/articles/202403-01t-azure_cloudshell_add_tag"
tags: [" #Zenn #2024-03 "]
aliases: 記事「Azure CloudSHellで使用するストレージにタグを追加する」
---

## はじめに

Azure Cloud Shell で使用するストレージアカウントに任意のタグを付与します。

## 結論

下記コマンドをAzure Cloud Shell(bash)で実行するとAzure Cloud Shellで使用しているストレージアカウントにタグが追加されます。
Azure Cloud Shellで使用しているストレージアカウントは、環境変数から取得するため一意です。

```bash
tag_var="owner=roota@example.com"
az tag update --resource-id $(echo $ACC_STORAGE_PROFILE | jq -r .storageAccountResourceId) \
              --operation merge \
              --tags $tag_var
```
※例ではownerという名前のタグにユーザーのメールアドレスをタグの値として設定しています
:::details 実行例

```bash
r_ota [ ~/clouddrive ]$ az tag update --resource-id $(echo $ACC_STORAGE_PROFILE | jq -r .storageAccountResourceId) \
              --operation merge \
              --tags $tag_var
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/cloud-shell-storage-southeastasia/providers/Microsoft.Storage/storageAccounts/cs123456789abcdef01/providers/Microsoft.Resources/tags/default",
  "name": "default",
  "properties": {
    "tags": {
      "ms-resource-usage": "azure-cloud-shell",
      "owner": "roota@example.com"
    }
  },
  "resourceGroup": "cloud-shell-storage-southeastasia",
  "type": "Microsoft.Resources/tags"
}
r_ota [ ~/clouddrive ]$ 
```
:::

## なぜこれをやりたかったのか？

Azure Cloud Shellの仕様で自動生成されたストレージアカウントは、ユーザー共通で同じリソースグループ内に作成され、どのストレージアカウントがどのユーザーに紐づいているか分かりにくいためです。

>基本設定を使用し、サブスクリプションのみを選択すると、Cloud Shell では、最寄りのサポートされるリージョンに 3 つのリソースが自動的に作成されます。
>
>リソース グループ: cloud-shell-storage-<region>
>ストレージ アカウント: cs<uniqueGuid>
>ファイル共有: cs-<user>-<domain>-com-<uniqueGuid>

引用元

@[card](https://learn.microsoft.com/ja-jp/azure/cloud-shell/persisting-shell-storage#create-new-storage)

そのため、任意のタグを付与すればわかりやすいと思ったので調べました。

実行ユーザーが不明のストレージアカウントが大量に発生・・
![](/images/202403-01t-azure_cloudshell_add_tag/202403-01t-azure_cloudshell_add_tag_001.png)

## 処理内容

1. Azure Cloud Shellで使用されているストレージアカウントを環境変数から特定
1. 特定したストレージアカウントにタグを追加

:::details 処理詳細
- $ACC_STORAGE_PROFILE
  `$ACC_STORAGE_PROFILE` は、環境変数で設定されている値です。格納されている値は下記の通りです。
  - storageAccountResourceId
  - fileShareName
  - diskSizeInGB
  出力例
  ```bash
  r_ota [ ~/clouddrive ]$ echo $ACC_STORAGE_PROFILE | jq .
  {
    "storageAccountResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/cloud-shell-storage-southeastasia/providers/Microsoft.Storage/storageAccounts/cs123456789abcdef01",
    "fileShareName": "cs-r-ota-example-com-23456789abcdef01",
    "diskSizeInGB": 5
  }
  r_ota [ ~/clouddrive ]$ 
  ```
  → `storageAccountResourceId` のみ取り出すために `| jq -r .storageAccountResourceId` を追加します。
- az tag update
  - [az tag update](https://learn.microsoft.com/ja-jp/cli/azure/tag?view=azure-cli-latest#az-tag-update) に `--operation merge`を追加して実行します。
  - [az tag create](https://learn.microsoft.com/ja-jp/cli/azure/tag?view=azure-cli-latest#az-tag-create) では、既存の ` "ms-resource-usage": "azure-cloud-shell"`が消えてしまうため、`az tag update` を使用して既存の値に追加します。
  ```bash
  az tag update --operation {Delete, Merge, Replace}
                --resource-id
                --tags
  ```
:::

## 感想

- Azureのユーザーが削除されてもCloud Shellで使用するストレージアカウントは紐づいて自動で削除されない？  
  →消し忘れ防止/リソース管理の観点からもタグ付与は有効だと感じました。
- 各ユーザーがCloud Shell上で実行しないといけないのが、イケてないと思いました。。改善の余地あり。
