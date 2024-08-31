---
title: "az network public-ip"
date: 2024-08-31 21:38:08
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-network-public-ip"
tags:
  - #yyyy-mm/2024-08
---

## コマンドリファレンス

- @[card](https://learn.microsoft.com/ja-jp/cli/azure/network/public-ip?view=azure-cli-latest)

## [az network public-ip list](https://learn.microsoft.com/ja-jp/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-list)

### パブリックIP一覧出力(tsv形式)

```bash
az network public-ip list --output tsv --query [].[name,resourceGroup,ipAddress,location]
```

### パブリックIP一覧出力(JSON形式)

```bash
az network public-ip list --output json > "${DIR}/az_network_public-ip_list.json"
```
