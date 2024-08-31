---
title: "az storage"
date: 2022-11-30
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-storage"
tags:
  - "#yyyy-mm/2022-11"
---

## コマンドリファレンス

- @[card](https://learn.microsoft.com/ja-jp/cli/azure/storage?view=azure-cli-latest)

## [az storage account list](https://learn.microsoft.com/ja-jp/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list)

### ストレージアカウント一覧取得(json形式)

```bash
az storage account list > "${DIR}/az_storage_account_list.json"
```

### ストレージアカウント一覧取得(csv形式)

```bash
echo '"resourceGroup","name","primaryLocation","sku.name","kind","accessTier","creationTime"' \
    > "${DIR}/az_storage_account_list.csv"
az storage account list \
    | jq -r '.[] | [.resourceGroup,
                    .name,
                    .primaryLocation,
                    .sku.name,
                    .kind,
                    .accessTier,
                    .creationTime]|@csv' \
    >> "${DIR}/az_storage_account_list.csv"
```

### ストレージアカウント一覧取得(csv出力/RESOURCE_GROUP指定)

```bash
echo '"resourceGroup","name","primaryLocation","sku.name","kind","accessTier","creationTime"' \
    > "${DIR}/az_storage_account_list_${RESOURCE_GROUP}.csv"
az storage account list \
    --resource-group $RESOURCE_GROUP \
    | jq -r '.[] | [.resourceGroup,
                    .name,
                    .primaryLocation,
                    .sku.name,
                    .kind,
                    .accessTier,
                    .creationTime]|@csv' \
    >> "${DIR}/az_storage_account_list_${RESOURCE_GROUP}.csv"
```

## [az storage account create](https://learn.microsoft.com/ja-jp/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)

### ストレージアカウント作成

```bash
STORAGE_ACCOUNT_NAME="storage$RANDOM"
```

```bash
az storage account create \
    --name $STORAGE_ACCOUNT_NAME \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --sku Standard_LRS \
    --kind StorageV2
```

## [az storage account network-rule list](https://learn.microsoft.com/ja-jp/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list)

### ストレージアカウント アクセス許可IPリスト表示(csv形式)

```bash
az storage account network-rule list \
    --account-name $STORAGE_ACCOUNT_NAME \
    | jq -r '.ipRules[] | [.action,.ipAddressOrRange] | @csv'
```


:::details 出力例

```bash
r_ota [ ~/clouddrive ]$ az storage account network-rule list --account-name $STORAGE_ACCOUNT_NAME | jq -r '.ipRules[] | [.action,.ipAddressOrRange] | @csv'
"Allow","13.73.248.16/29"
"Allow","20.21.37.40/29"
"Allow","20.36.120.104/29"
＜中略＞
r_ota [ ~/clouddrive ]$ 
```

:::

:::message
参考
ストレージアカウント アクセス許可IPリスト は、Azureポータル上で下記のメニューに記載の内容です。

ストレージアカウント ＞ セキュリティとネットワーク ＞ ファイアウォールと仮想ネットワーク

![](https://storage.googleapis.com/zenn-user-upload/a24f8b0af299-20230427.png)

:::

### ストレージアカウント アクセス許可IPリスト出力(csv形式)

```bash
echo '"action","ipAddressOrRange"' \
    > "${DIR}/az_storage_account_network-rule_list_$STORAGE_ACCOUNT_NAME.csv"

az storage account network-rule list \
    --account-name $STORAGE_ACCOUNT_NAME \
    | jq -r '.ipRules[] | [.action,.ipAddressOrRange] | @csv' \
    >> "${DIR}/az_storage_account_network-rule_list_$STORAGE_ACCOUNT_NAME.csv"
```
