---
title: "[小ネタ] Azure Cloud Shellで使用するストレージにタグを追加する"
emoji: "🎈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [azure,cloudshell,小ネタ]
published: false
publication_name: "ap_com"

date: 2024-03-03
url: "https://zenn.dev/roota5666/articles/202403-01t-azure_cloudshell_add_tag"
tags: [" #Zenn #2024-03 "]
aliases: 記事「[小ネタ] Azure CloudSHellで使用するストレージにタグを追加する」
---

## はじめに

Azure Cloud Shell で使用するストレージアカウントに任意のタグを付与する。

## 結論

下記コマンドをAzure Cloud Shell(bash)で実行するとCloud Shellで使用しているストレージアカウントにタグを付与できます。

```bash
tag_var=owner=roota@example.com
az tag update --resource-id $(echo $ACC_STORAGE_PROFILE | jq -r .storageAccountResourceId) \
              --operation merge \
              --tags $tag_var
```
:::details 実行例

```bash
r_ota [ ~/clouddrive ]$ az tag create --resource-id $(az resource show --ids $(echo $ACC_STORAGE_PROFILE | jq -r .storageAccountResourceId) | jq -r .id) --tags $tag_var ms-resource-usage=azure-cloud-shell
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

## 環境

- Azure Cloud Shell

## なぜこれをやりたかったのか？

Azure Cloud Shellの仕様で自動生成されたストレージアカウントは、ユーザー共通で同じリソースグループ内に作成され、
どのストレージアカウントがどのユーザーに紐づいているか分かりにくい。

>基本設定を使用し、サブスクリプションのみを選択すると、Cloud Shell では、最寄りのサポートされるリージョンに 3 つのリソースが自動的に作成されます。
>
>リソース グループ: cloud-shell-storage-<region>
>ストレージ アカウント: cs<uniqueGuid>
>ファイル共有: cs-<user>-<domain>-com-<uniqueGuid>

引用元：<https://learn.microsoft.com/ja-jp/azure/cloud-shell/persisting-shell-storage#create-new-storage>

そのため、任意のタグを付与すればわかりやすいと思ったので調べた。

## 実施していること

### $ACC_STORAGE_PROFILE とは

`$ACC_STORAGE_PROFILE` は、環境変数で設定されている値

```bash
r_ota [ ~/clouddrive ]$ echo $ACC_STORAGE_PROFILE | jq .
{
  "storageAccountResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/cloud-shell-storage-southeastasia/providers/Microsoft.Storage/storageAccounts/cs123456789abcdef01",
  "fileShareName": "cs-r-ota-example-com-23456789abcdef01",
  "diskSizeInGB": 5
}
r_ota [ ~/clouddrive ]$ 
```

`storageAccountResourceId` のみ取り出すために `| jq -r .storageAccountResourceId` を追加する。

### az tag update

[az tag update](https://learn.microsoft.com/ja-jp/cli/azure/tag?view=azure-cli-latest#az-tag-update) に `--operation merge`を追加して実行する。
[az tag create](https://learn.microsoft.com/ja-jp/cli/azure/tag?view=azure-cli-latest#az-tag-create) だと既存の ` "ms-resource-usage": "azure-cloud-shell"`が消えてしまうため、`az tag update` を使用する。

```bash
az tag update --operation {Delete, Merge, Replace}
              --resource-id
              --tags
```

## 感想

各ユーザーがCloud Shell上で実行しないといけないのが、イケてないと思った。
ないよりはマシ・・？
Azureのユーザーが削除されてもCloud Shellで使用するストレージアカウントは紐づいて自動で削除されないはずなので、
消し忘れ防止/リソース管理の観点からもタグ付与は有効だと思いました。

