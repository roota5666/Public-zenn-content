---
filename: 20241208-01t-azurexterraform-subscriptionid
title: "AzureでTerraformを久しぶりに実行したらエラーになった件"
emoji: "😖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ azure,terraform]
published: true # published_at: "{{date:YYYY-MM-DD}}" # publication_name: "ap_com"

url: "https://zenn.dev/roota5666/articles/20241208-01t-azurexterraform-subscriptionid"
tags:
  - "#web/site/zenn"
aliases:
  - 記事「AzureでTerraformを久しぶりに実行したらエラーになった件」
---

## はじめに

こちらは [エーピーコミュニケーションズ Advent Calendar 2024](https://qiita.com/advent-calendar/2024/ap-com) の 8 日目の記事となります。

## 結論

AzureでCloud ShellからTerraformを久しぶりに実行したらエラーになりました。
結論としては、azurerm providerでsubscription_idの指定が必要でした。

```diff
provider "azurerm" {
  features {}
+  subscription_id = "12345abc-1234-abcd-1234-12345abcdef1"
}
```

## 環境

- :::details Terraform v1.9.5

  ```bash
  r_ota [ ~ ]$ terraform -v 
  Terraform v1.9.5
  on linux_amd64
  + provider registry.terraform.io/hashicorp/azurerm v3.112.0
  
  Your version of Terraform is out of date! The latest version
  is 1.10.1. You can update by downloading from https://www.terraform.io/downloads.html
  r_ota [ ~ ]$ 
  ```

  :::

- :::details azure-cli 2.65.0

  ```bash
  r_ota [ ~ ]$ az --version
  azure-cli                         2.65.0 *
  
  core                              2.65.0 *
  telemetry                          1.1.0
  
  Extensions:
  ai-examples                        0.2.5
  ml                                2.30.1
  ssh                                2.0.5
  
  Dependencies:
  msal                              1.31.0
  azure-mgmt-resource               23.1.1
  
  Python location '/usr/bin/python3.9'
  Extensions directory '/home/r_ota/.azure/cliextensions'
  Extensions system directory '/usr/lib/python3.9/site-packages/azure-cli-extensions'
  
  Python (Linux) 3.9.19 (main, Aug 23 2024, 00:07:48) 
  [GCC 11.2.0]
  
  Legal docs and information: aka.ms/AzureCliLegal
  
  
  You have 2 update(s) available. They will be updated with the next build of Cloud Shell.
  r_ota [ ~ ]$ 
  ```

  :::

## 事象と解決

### やったこと

下記のtfファイルを使用して、`terraform init` `terraform plan` するとエラーがでました。

https://github.com/roota5666/book-contents_azure-x-terraform/blob/main/lesson2/main.tf

- 実行エラー

  ```bash
  r_ota [ ~ ]$ terraform plan
  
  Planning failed. Terraform encountered an error while generating this plan.
  
  ╷
  │ Error: `subscription_id` is a required provider property when performing a plan/apply operation
  │ 
  │   with provider["registry.terraform.io/hashicorp/azurerm"],
  │   on main.tf line 8, in provider "azurerm":
  │    8: provider "azurerm" {
  │ 
  ╵
  r_ota [ ~ ]$ 
  ```

### エラー内容

書くまでもないですが・・

>Error: `subscription_id` is a required provider property when performing a plan/apply operation

訳　エラー：`subscription_id` は、計画/申請操作の実行時に必須のプロバイダー・プロパティです。

### 変更点

subscription_id を追加しました。

```diff
provider "azurerm" {
  features {}
+  subscription_id = "12345abc-1234-abcd-1234-12345abcdef1"
}
```

> :::message
> subscription_idを表示させる一例
> Azure CLIで下記コマンドを実行する
>
> ```bash
> r_ota [ ~ ]$  az account list --output json | jq -r .[].id
> 12345abc-1234-abcd-1234-12345abcdef1
> r_ota [ ~ ]$
> ```

### 変更箇所調査

変更箇所がどの時点のバージョンから変更されたか？を調査しましたが、「コレ！」というのは見つけられませんでした・・

- [Terraform AzureRM provider version history: 4.0.0 - current](https://learn.microsoft.com/en-us/azure/developer/terraform/provider-version-history-azurerm-4-0-0-to-current)
- [bugfix: provider validation for subscription_id #27178](https://github.com/hashicorp/terraform-provider-azurerm/pull/27178)

## 感想&まとめ

- 以前動いたコードが動かなくなること・・・ はたまにあること、ですが久しぶりに当たってちょっとハマりました。。
- エラー文がわかりやすくてよかったです。
- AzurexTerraform は情報が少ない印象です。。

最後までお読みいただきありがとうございました。

## 参考サイト

- @[card](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/subscription)

## 宣伝

AzurexTerraformのZenn本です。こちらもよろしくお願いします。

- @[card](https://zenn.dev/roota5666/books/azure-x-terraform)
