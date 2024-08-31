---
title: "az account"
date: 2023-04-25
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-account"
tags:
  - #yyyy-mm/2023-04
---

## コマンドリファレンス

- @[card](https://learn.microsoft.com/ja-jp/cli/azure/account?view=azure-cli-latest)

## [az account list](https://learn.microsoft.com/ja-jp/cli/azure/account?view=azure-cli-latest#az-account-list)

### アカウントリスト表示(json形式)

```bash
az account list --output json
```

:::message
コラム
PowerShellの場合は、下記コマンドで同等の結果を取得できます。

```powershell
Get-AzSubscription
```

リファレンス:[Get-AzSubscription](https://learn.microsoft.com/ja-jp/powershell/module/az.accounts/get-azsubscription?view=azps-9.6.0)
:::
