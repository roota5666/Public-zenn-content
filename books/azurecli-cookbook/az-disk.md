---
title: "az disk"
date: 2022-11-30
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-disk"
tags:
  - "#yyyy-mm/2022-11"
---

## コマンドリファレンス

- @[card](https://docs.microsoft.com/ja-jp/cli/azure/disk?view=azure-cli-latest)

## [az disk list](https://learn.microsoft.com/ja-jp/cli/azure/disk?view=azure-cli-latest#az-disk-list)

### ディスク一覧表示(tsv)

```bash
az disk list --output tsv --query [].[location,name,resourceGroup,sku.name]
```

### ディスク一覧取得(json形式)

```bash
az disk list --output json > "${DIR}/az_disk_list.json"
```

## [az disk show ](https://learn.microsoft.com/ja-jp/cli/azure/disk?view=azure-cli-latest#az-disk-show)

### ディスク詳細取得
