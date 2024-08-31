---
title: "はじめに"
date: 2022-10-29
url: "https://zenn.dev/roota5666/books/azure-x-terraform/viewer/001-introduction"
tags:
  - "#yyyy-mm/2022-10"
  - "#web/site/zenn/book"
  - "#tech/terraform"
---

## この本について

Terraformを1ミリも触ったことない自分の学習用に作成しました。
のちに、誰かのために役に立つコンテンツに昇華できたらいいなぁ・・

更新は不定期です。

## 環境

- Azure
  - Azure Cloud Shell
    - Azure CLI*
    - Terraform*

\* Azure Cloud Shellで提供されるバージョンを使用します

## 教材

下記のGithubリポジトリに対応した教材ファイルを格納します。
`git clone` してお使いください。

<https://github.com/roota5666/book-contents_azure-x-terraform>

```bash
mkdir clouddrive/git
cd clouddrive/git
git clone https://github.com/roota5666/book-contents_azure-x-terraform.git
cd book-contents_azure-x-terraform
```

## よく使うコマンド

作った(apply)ら、確認して(state list)、消す(destroy)

```bash
terraform init
```

```bash
terraform plan
```

```bash
terraform apply
```

```bash
terraform destroy
```

```bash
terraform state list
```
