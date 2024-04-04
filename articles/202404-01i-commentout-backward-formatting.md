---
title: "行末コメントアウトのインデントをそろえる"
emoji: "🖥️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [ "vscode","文字列整形"]
published: false

date: 2024-04-04
url: "<https://zenn.dev/roota5666/articles/202404-01i-commentout-backward-formatting>"
tags: ["#Zenn #2024-04"]
aliases: 記事「行末コメントアウトのインデントをそろえる」
---

## はじめに

本記事では、行末コメントアウトのインデントをそろえる方法について記載します。  
yamlファイルなど、行末にコメントを記述できるファイルで「インデントそろえたいけど、スペースキー連打はツラい！！」  
というときに調べた結果、たどりついた操作方法について記載します。

## 環境

- Visual Studio Code

## やりたいこと

発端のTweet

@[tweet](https://twitter.com/roota5666/status/1651568805838192640?s=20)

これを↓

![](/images/202404-01i-commentout-backward-formatting/202404-01i-commentout-backward-formatting_001_before.png)

こうしたい↓

![](/images/202404-01i-commentout-backward-formatting/202404-01i-commentout-backward-formatting_002_after.png)

:::details 文字列としては、こんな感じ

- before

  ```text
  az account                 # Azure サブスクリプション情報を管理します。
  az acr                 # Azure Container Registries を使用してプライベート レジストリを管理します。
  az ad                 # ロール ベースのAccess Controlに必要な Azure Active Directory Graph エンティティを管理します。
  az adp                 # Azure Autonomous Development Platform リソースを管理します。
  az advisor                 # Azure Advisor を管理します。
  az afd                 # Azure Front Door Standard/Premium を管理します。 従来の Azure Front Door については、 を参照してください https://docs.microsoft.com/en-us/cli/azure/network/front-door?view=azure-cli-latest。
  az ai-examples                 # AI を使用した例をヘルプ コンテンツに追加します。
  az aks                 # Azure Kubernetes Service を管理します。
  az alias                 # Azure CLI エイリアスを管理する。
  az ams                 # Azure Media Services リソースを管理します。
  az apim                 # Azure API Management サービスを管理する。
  az appconfig                 # アプリ構成を管理する。
  az appservice                 # App Service プランを管理します。
  az arcappliance                 # アプライアンス リソースを管理するコマンド。
  az arcdata                 # Azure Arc 対応データ サービスを使用するためのコマンド。
  az aro                 # Azure Red Hat OpenShift クラスターを管理します。
  az artifacts                 # Azure Artifacts を管理します。
  az attestation                 # Microsoft Azure Attestation (MAA) を管理します。
  az automanage                 # Automanage を管理します。
  az automation                 # Automation アカウントを管理します。
  ```

- after

  ```text
  az account          # Azure サブスクリプション情報を管理します。
  az acr              # Azure Container Registries を使用してプライベート レジストリを管理します。
  az ad               # ロール ベースのAccess Controlに必要な Azure Active Directory Graph エンティティを管理します。
  az adp              # Azure Autonomous Development Platform リソースを管理します。
  az advisor          # Azure Advisor を管理します。
  az afd              # Azure Front Door Standard/Premium を管理します。 従来の Azure Front Door については、 を参照してください https://docs.microsoft.com/en-us/cli/azure/network/front-door?view=azure-cli-latest。
  az ai-examples      # AI を使用した例をヘルプ コンテンツに追加します。
  az aks              # Azure Kubernetes Service を管理します。
  az alias            # Azure CLI エイリアスを管理する。
  az ams              # Azure Media Services リソースを管理します。
  az apim             # Azure API Management サービスを管理する。
  az appconfig        # アプリ構成を管理する。
  az appservice       # App Service プランを管理します。
  az arcappliance     # アプライアンス リソースを管理するコマンド。
  az arcdata          # Azure Arc 対応データ サービスを使用するためのコマンド。
  az aro              # Azure Red Hat OpenShift クラスターを管理します。
  az artifacts        # Azure Artifacts を管理します。
  az attestation      # Microsoft Azure Attestation (MAA) を管理します。
  az automanage       # Automanage を管理します。
  az automation       # Automation アカウントを管理します。
  ```

:::

## 結論(手順)

| #   | 動作                             | キー操作           |
| :-- | :------------------------------- | :----------------- |
| 1   | コメントを揃えたい位置で矩形選択 | [Ctrl]+[Alt]+[↓]   |
| 2   | # まで選択                       | [Ctrl]+[Shift]+[→] |
| 3   | # の1つ前まで選択                | [Shift]+[←]        |
| 4   | 削除                             | [Del]              |
| 5   | 矩形選択解除                     | [Esc]              |

![](/images/202404-01i-commentout-backward-formatting/202404-01i-commentout-backward-formatting_003.gif)

## 感想

見つけると/知っていると「余裕だぜ！」ってなるんですが、知るまでに苦労しますよね・・・  
試行錯誤、特に今回はどうやって検索したら出るのかが分かりませんでした。。  
教えていただいた、@atakamo_sonna さん、ありがとうございました。

@[tweet](https://twitter.com/atakamo_sonna/status/1651623166656266240?s=20)
