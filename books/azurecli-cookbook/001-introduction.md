---
title: "はじめに"
date: 2022-11-30
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/001-introduction"
tags:
  - #yyyy-mm/2022-11
  - #web/site/zenn/book
  - #tech/azure
---

## まえがき

本書は、Azureを使用している筆者が「Azureポータルからもできるけど、コマンドで実行したほうが楽だなー」と思い、Azure Cloud Shellを使用してAzure CLIでいろいろやってみた！を本にしたものです。

## 本書の対象者

- Azure CLIを使ったことがある人/これからさわってみたい人
- AzureポータルからGUIポチポチがダルイと感じる人
- 自動化や定期実行をやってみたい人

## 本書の内容

- Azure CLIを使用したコマンドの記載
- Azure CLIの簡単な解説
- Azure CLIコマンドと同等の情報取得または変更ができるAzure PowerShellコマンドの紹介をたまに・・
- コラムと称した雑記

## 本書の構成

[az reference](https://docs.microsoft.com/ja-jp/cli/azure/reference-index?view=azure-cli-latest)に記載のコマンドをアルファベット順にページを作成して記載します。

## 本書の読み方

### 本書で使用する変数について

本書では、汎用性を高めるため、また本家Microsoftと記述レベルを合わせるため、記載のコマンドにはなるべく変数を使用します。

|種別|変数|内容|参考値|
|:----|:----|:----|:----|
|本書独自|DIR |出力先ディレクトリ||
|本書独自|MAIL_ADDRESS |メールアドレス||
|共通|RESOURCE_GROUP |リソースグループ||
|共通|LOCATION |場所|westus2<br>southcentralus<br>centralus<br>eastus<br>westeurope<br>southeastasia<br>japaneast<br>brazilsouth<br>australiasoutheast<br>centralindia|
|storage|STORAGE_ACCOUNT_NAME |ストレージアカウント||
|storage|STORAGE_ACCOUNT_RESOURCEID |ストレージアカウント リソースID||
|cdn|CDN_PROFILE_NAME |CDNプロファイル名||

:::details 使用例

実行コマンド

```bash
DIR=./export
mkdir export
az vm list > "${DIR}/az_vm_list.json"
ls -l $DIR
```

出力例

```bash
r_ota [ ~/clouddrive ]$ DIR=./export
r_ota [ ~/clouddrive ]$ mkdir export
r_ota [ ~/clouddrive ]$ az vm list > "${DIR}/az_vm_list.json"
r_ota [ ~/clouddrive ]$ ls -l $DIR
total 22
-rwxrwxrwx 1 r_ota r_ota 22268 May  4 17:14 az_vm_list.json
r_ota [ ~/clouddrive ]$ 
```

:::

## 本書のこだわり

### オプションは略さない

例えば`az vm list`では、ResourceGroup指定で `--resource-group` または `-g` で指定できます。
下記2つは同じ結果が得られます。

```bash:パターン1
az vm list --resource-group MyResourceGroup
```

```bash:パターン2
az vm list -g MyResourceGroup
```

本書では、パターン1の`--resource-group`のように表記が長くて具体的な方を採用/記載します。

:::message
コラム

有名どころ(？)だとciscoのconf t

```bash
configure terminal
```

```bash
conf t
```

Powershellのgciなどが、表記の短縮が可能、またはaliasですね。

```powershell
PS C:\> Get-Alias gci

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gci -> Get-ChildItem


PS C:\>
```

私も手癖で打っちゃうのですが、昔先輩から「手順書などに記載する際は略さないで書いたほうがいい」と教わったので、本書でも長い方のオプション指定で分かりやすく記載していきます。

:::

### 長いコマンドは分割して表示する

折り返しされているコマンドは、スクロールバーをクリックして端までもっていかないといけなく、可読性が悪いと感じます。

```bash:パータンα
az keyvault create --name $KEYVAULT_NAME --resource-group $RESOURCE_GROUP --location $LOCATION --sku standard
```

```bash:パータンβ
az keyvault create \
    --name $KEYVAULT_NAME \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --sku standard
```

引用元：[Azure Key Vault サービスを作成する](https://learn.microsoft.com/ja-jp/training/modules/secure-application-secrets-use-key-vault/2-create-azure-service)

そのため、本書ではパータンβのように1行のコマンドでも複数行に表示上改行して記載します。

## 注意

- 本書の更新は気まぐれです
- 使用しているコマンドはほんの一部になります
