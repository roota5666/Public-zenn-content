---
title: "ansible ping モジュールを使った疎通確認"
emoji: "🅰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ansible"]
published: true
publication_name: "ap_com"
url: "https://zenn.dev/roota5666/articles/202506-01t-ansibleping"
author: roota5666
description: "ansible ping モジュールを使った疎通確認でansible-playbookコマンドでplaybookを指定した場合と、直接ansibleコマンドでmoduleを指定した場合の出力結果の見やすさにフォーカスして結果を比較してみた"
---

## 👀シチュエーション

ansibleを使っていると

- インベントリファイルにターゲットホストを追加したので疎通確認したい
- 設定変更や情報取得処理の前に接続有無を確認したい

ことがよくあります。(あるんです！)

そんな時に [ansible.builtin.ping module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html) を使います。
しかし、毎回「どうやって使うんだっけ(-ω-;)」 「見やすい出力は(・・?」となるので、改めて検証してみました。

## ⚙️環境

- ansible [core 2.18.4]

## 🧪検証

### 1. ansibleコマンドで直接指定

- 実行内容
  `ansible` コマンドで直接インベントリファイル/実行するmoduleを指定します。  
メリットはping用のplaybookをわざわざ作成しなくてもよい点です。

:::details テスト用インベントリファイル

ターゲットホストを準備できなかったため、localhost x 5 のインベントリファイルを使用

```yaml
all:
  hosts:
    localhost1:
      ansible_host: 127.0.0.1
      ansible_connection: local
    localhost2:
      ansible_host: 127.0.0.1
      ansible_connection: local
    localhost3:
      ansible_host: 127.0.0.1
      ansible_connection: local
    localhost4:
      ansible_host: 127.0.0.1
      ansible_connection: local
    localhost5:
      ansible_host: 127.0.0.1
      ansible_connection: local
  vars:
    ansible_python_interpreter: /usr/bin/python3
```

:::

- 実行例

  ```bash
  ansible -i host.yml ,all -m ansible.builtin.ping
  ```

- 出力例

  ```bash
  r_ota [ ~ ]$ ansible -i host.yml ,all -m ansible.builtin.ping
  localhost4 | SUCCESS => {
      "changed": false,
      "ping": "pong"
  }
  localhost5 | SUCCESS => {
      "changed": false,
      "ping": "pong"
  }
  localhost1 | SUCCESS => {
      "changed": false,
      "ping": "pong"
  }
  localhost2 | SUCCESS => {
      "changed": false,
      "ping": "pong"
  }
  localhost3 | SUCCESS => {
      "changed": false,
      "ping": "pong"
  }
  r_ota [ ~ ]$ 
  ```

- 結果 -> 複数行で結果が出力され、5対象くらいなら目視で確認できますが10対象超えるとツラそう・・・

### 2. ansible-playbookコマンドでplaybookを実行する

- 実行内容
  `ansible-playbook` インベントリファイルとping moduleを記述したplaybookを実行します。

:::details ping moduleを記述したplaybook

```yaml
---
- hosts: all
  gather_facts: false

  tasks:
    - name: ping
      ansible.builtin.ping:
      ignore_errors: yes
```

:::


- 実行例

  ```bash
  ansible-playbook -i host.yml ping.yml 
  ```

- 出力例

  ```bash

  ```bash
  r_ota [ ~ ]$ ansible-playbook -i host.yml ping.yml 

  PLAY [all] **********************************************************************************************************************************************************************************************************************************

  TASK [ping] *********************************************************************************************************************************************************************************************************************************
  ok: [localhost3]
  ok: [localhost5]
  ok: [localhost1]
  ok: [localhost4]
  ok: [localhost2]

  PLAY RECAP **********************************************************************************************************************************************************************************************************************************
  localhost1                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  localhost2                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  localhost3                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  localhost4                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  localhost5                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

  r_ota [ ~ ]$ 
  ```

- 結果 -> PLAY RECAPが表示されてOKの対象数も数えやすい！イイ！

## 🎉結論

ターゲットホストが多いときに、ansibleでping moduleを使う際は

```bash
ansible-playbook -i 【インベントリファイル】 【ping module用プレイブック】
```

がPLAY RECAPが表示されて見やすい！

## ✅感想

なにかと使う疎通確認。だけど毎回「えっ？エラー！？なんだっけ... allつけるんだっけ...」わからん！あと、結果が見にくい！！となっていたので、今回検証してみて理解が深まりました。

最後までお読みいただきありがとうございました。

## 🔗参考サイト

@[card](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html)
