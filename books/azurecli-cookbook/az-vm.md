---
title: "az vm"
date: 2022-11-30
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-vm"
tags:
  - #yyyy-mm/2022-11
---

## コマンドリファレンス

- @[card](https://docs.microsoft.com/ja-jp/cli/azure/vm?view=azure-cli-latest)

## [az vm list](https://learn.microsoft.com/ja-jp/cli/azure/vm?view=azure-cli-latest#az-vm-list)

### VMリスト表示(tsv形式)

```bash
az vm list --output tsv --query [].[location,name,resourceGroup,storageProfile.osDisk.osType]
```

### VMリスト出力(json形式)

```bash
az vm list --output json > "${DIR}/az_vm_list.json"
```
