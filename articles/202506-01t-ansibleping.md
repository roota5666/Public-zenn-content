---
title: "ansible ping モジュールを使った疎通確認"
emoji: "🅰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ansible"]
published: false
publication_name: "ap_com"
url: "https://zenn.dev/roota5666/articles/202506-01t-ansibleping"
author: roota5666
description: "ansible ping モジュールを使った疎通確認でansible-playbookコマンドでplaybookを指定した場合と、直接ansibleコマンドでmoduleを指定した場合の出力結果の見やすさにフォーカスして結果を比較してみた"
---

## 🎉結論

ターゲットホストが多いときに、ansibleでping moduleを使う際は

```bash
ansible-playbook -i 【インベントリファイル】 【ping module用プレイブック】
```

が見やすい！

## ⚙️環境

- ansible [core 2.18.4]

## 👀シチュエーション

ansibleを使っていると

- インベントリファイルにターゲットホストを追加したので疎通確認したい
- 設定変更や情報取得処理の前に接続有無を確認したい

ことがよくあります。(あるんです！)

そんな時に [ansible.builtin.ping module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html) を使います。

:::details ansible.builtin.ping module 概要

このモジュールは単純なテストモジュールで、pong接続に成功すると常に戻り値を返します。プレイブックでは意味がありませんが、/usr/bin/ansibleログインできることと、使用可能なPythonが設定されていることを確認するのに役立ちます。

これは ICMP ping ではなく、リモート ノードで Python を必要とする単純なテスト モジュールです。

Windows ターゲットの場合は、代わりにansible.windows.win_pingモジュールを使用します。

ネットワーク ターゲットの場合は、代わりにansible.netcommon.net_pingモジュールを使用します。

---

A trivial test module, this module always returns pong on successful contact. It does not make sense in playbooks, but it is useful from /usr/bin/ansible to verify the ability to login and that a usable Python is configured.

This is NOT ICMP ping, this is just a trivial test module that requires Python on the remote-node.

For Windows targets, use the ansible.windows.win_ping module instead.

For Network targets, use the ansible.netcommon.net_ping module instead.

:::

## 🧪検証

### 1. ansibleコマンドで直接指定

`ansible` コマンドで直接インベントリファイル/実行するmoduleを指定します。

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

  -> 複数行で結果が出力され、5対象くらいなら目視で確認できますが10対象超えるとツラそう・・・

### 2. ansible-playbookコマンドでplaybookを実行する

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

  -> PLAY RECAPが表示されてOKの対象数も数えやすい！イイ！

### 🎉結論(再掲)

ターゲットホストが多いときに、ansibleでping moduleを使う際は

```bash
ansible-playbook -i 【インベントリファイル】 【ping module用プレイブック】
```

がPLAY RECAPが表示されて見やすい！

## ✅感想

なにかと使う疎通確認。だけど毎回「えっ？エラー！？なんだっけ... allつけるんだっけ...」わからん！あと、結果が見にくい！！となっていたので、今回まとめてみました。

最後までお読みいただきありがとうございました。

## 🔗参考サイト

@[card](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html)
