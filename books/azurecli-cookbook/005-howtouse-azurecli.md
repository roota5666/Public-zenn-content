---
title: "Azure CLIの使い方"
date: 2023-04-26
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/howtouse-azurecli"
tags:
  - "#yyyy-mm/2023-04"
---

## Azure CLIとは？

>Azure コマンド ライン インターフェイス (Azure CLI) は、Azure リソースを作成および管理するためのコマンド セットです。 Azure CLI は Azure サービス全体で利用でき、オートメーションに重点を置くことで、Azure で迅速に作業できるように設計されています。

引用元：[Azure コマンド ライン インターフェイス (CLI) ドキュメント](https://learn.microsoft.com/ja-jp/cli/azure/)

:::message
注意
本書では、Microsoft ドキュメント サイトと同じく、Azure CLI の例は `bash` シェルで記述します
:::

## Azure Cloud Shellとは？

>Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーでアクセスできる対話形式の認証されたターミナルです。 Bash または PowerShell どちらかのシェル エクスペリエンスを作業方法に合わせて柔軟に選択できます。

引用元：[Azure Cloud Shell の概要](https://learn.microsoft.com/ja-jp/azure/cloud-shell/overview)

### Azure Cloud Shellの使い方

使用方法はいくつかありますが、3つ記載します。

- [portal.azure.com](https://portal.azure.com/)
  →URLにアクセスすると、Azureポータルが起動します。画面右上の`CloudShell`ボタンをクリックしてください。
- [shell.azure.com](https://shell.azure.com/)
  →URLにアクセスすると、Azureポータル起動後すぐにAzure Cloud Shellが起動します。
- Windows11以降、ターミナルからAzure Cloud Shellを起動
  →Terminalを起動、Azure Cloud Shellから起動できます。

:::message
参考

- @[card](https://learn.microsoft.com/ja-jp/azure/cloud-shell/quickstart?tabs=azurecli)
- Azure Cloud Shell in the Windows Terminal 💻
  @[youtube](https://www.youtube.com/watch?v=8CGOARw6zsk&ab_channel=ThomasMaurer)

:::

### Azure Cloud Shellのドライブ(ディスク)について

bashの場合は、下記コマンドで確認できます。

```bash
df -h
```

:::details コマンド結果

```bash
r_ota [ ~ ]$ df -h 
Filesystem                                                                          Size  Used Avail Use% Mounted on
overlay                                                                              49G   31G   18G  63% /
tmpfs                                                                                64M     0   64M   0% /dev
tmpfs                                                                               2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sdb1                                                                            49G   31G   18G  63% /home
shm                                                                                  64M     0   64M   0% /dev/shm
//cs110032001891f3055.file.core.windows.net/cs-r-ota-xxxxxxxxxxxx-10032001891f3055  6.0G  5.1G  1.0G  84% /usr/csuser/clouddrive
tmpfs                                                                               2.0G     0  2.0G   0% /proc/acpi
tmpfs                                                                               2.0G     0  2.0G   0% /proc/scsi
tmpfs                                                                               2.0G     0  2.0G   0% /sys/firmware
/dev/loop0                                                                          4.9G  755M  3.9G  17% /home/r_ota
r_ota [ ~ ]$ 
```

ストレージアカウント確認

![](https://storage.googleapis.com/zenn-user-upload/a6637598bb8a-20230507.png)

:::

:::message
参考
clouddrive の一覧取得
@[card](https://learn.microsoft.com/ja-jp/azure/cloud-shell/persisting-shell-storage#list-clouddrive)
:::
