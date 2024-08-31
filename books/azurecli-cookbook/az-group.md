---
title: "az group"
date: 2022-11-30
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-group"
tags:
  - #yyyy-mm/2022-11
---

## コマンドリファレンス

- @[card](https://docs.microsoft.com/ja-jp/cli/azure/group?view=azure-cli-latest)

## [az group list](https://learn.microsoft.com/ja-jp/cli/azure/group?view=azure-cli-latest#az-group-list)

### グループリスト表示(tsv形式/[地域,名前]のみ表示)

```bash
az group list --output tsv --query [].[location,name] 
```

### グループリスト表示(json形式)

```bash
az group list --output json
```

### グループリスト出力(jsonファイル)

```bash
az group list --output json > "${DIR}/az_group_list.json"
```

### グループリスト表示(json形式/グループ指定)

```bash
az group list --output json --query "[?name=='$RESOURCE_GROUP']"
```
