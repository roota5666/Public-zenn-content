---
title: "az cdn"
date: 2023-04-21
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-cdn"
tags:
  - "#yyyy-mm/2023-04"
---

## コマンドリファレンス

- @[card](https://learn.microsoft.com/ja-jp/cli/azure/cdn?view=azure-cli-latest)

## [az cdn profile list](https://learn.microsoft.com/ja-jp/cli/azure/cdn/profile?view=azure-cli-latest#az-cdn-profile-list)

### CDNプロファイルリスト出力(json形式)

```bash
az cdn profile list --output json > "${DIR}/az_cdn_profile_list.json"
```

## [az cdn endpoint list](https://learn.microsoft.com/ja-jp/cli/azure/cdn/endpoint?view=azure-cli-latest#az-cdn-endpoint-list)

### CDNエンドポイント一覧出力(json形式)

```bash
az cdn endpoint list --output json \
                     --profile-name $CDN_PROFILE_NAME \
                     --resource-group $RESOURCE_GROUP \
                     > "${DIR}/az_cdn_endpoint_list.json"
```

## [az cdn endpoint rule show](https://learn.microsoft.com/ja-jp/cli/azure/cdn/endpoint/rule?view=azure-cli-latest#az-cdn-endpoint-rule-show)

## [az cdn endpoint rule action show](https://learn.microsoft.com/ja-jp/cli/azure/cdn/endpoint/rule/action?view=azure-cli-latest#az-cdn-endpoint-rule-action-show)

## [az cdn edge-node list](https://learn.microsoft.com/ja-jp/cli/azure/cdn/edge-node?view=azure-cli-latest#az-cdn-edge-node-list)

### CDN/edge-node list 出力(json形式)

```bash
az cdn edge-node list --output json > "${DIR}/az_cdn_edge-node_list.json"
```

### CDN/SKU種別確認

```bash
az cdn edge-node list --output json | jq .[].name
```

:::details コマンド結果例

```bash
r_ota [ ~ ]$ az cdn edge-node list --output json | jq .[].name
"Standard_Verizon"
"Premium_Verizon"
"Custom_Verizon"
r_ota [ ~ ]$ 
```

:::

### CDN/edge-node list 出力(txt形式)

下記は`Azure CDN Premium from Verizon`の`edge-node list`取得例です。

```bash
CDN_SKU="Premium_Verizon"
az cdn edge-node list --output json \
    | jq -r '.[] | select(.name == "'${CDN_SKU}'")| 
    .ipAddressGroups[].ipv4Addresses' \
    | jq -r '.[] | [.baseIpAddress, .prefixLength]| @csv' \
    | sed -e 's/"//g' \
    | sed -e 's/,/\//g' \
    > "${DIR}/az_cdn_edge-node_list_${CDN_SKU}_ipv4Addresses.txt"
```

:::details 出力ファイル例

```bash
r_ota [ ~/clouddrive ]$ cat "${DIR}/az_cdn_edge-node_list_${CDN_SKU}_ipv4Addresses.txt"
5.104.64.0/21
46.22.64.0/20
61.221.181.64/26
68.232.32.0/20
72.21.80.0/20
88.194.45.128/26
93.184.208.0/20
101.226.203.0/24
108.161.240.0/20
110.232.176.0/22
117.18.232.0/21
117.103.183.0/24
120.132.137.0/25
121.156.59.224/27
121.189.46.0/23
152.195.0.0/16
180.240.184.0/24
192.16.0.0/18
192.30.0.0/19
192.229.128.0/17
194.255.210.64/26
198.7.16.0/20
203.74.4.64/26
213.64.234.0/26
213.65.58.0/24
68.140.206.0/23
68.130.0.0/17
152.190.247.0/24
65.222.137.0/26
65.222.145.128/26
65.198.79.64/26
65.199.146.192/26
65.200.151.160/27
65.200.157.192/27
68.130.128.0/24
68.130.136.0/21
65.200.46.128/26
213.175.80.0/24
152.199.0.0/16
36.67.255.152/29
194.255.242.160/27
195.67.219.64/27
88.194.47.224/27
203.66.205.0/24
110.164.36.0/24
119.46.85.0/24
49.231.126.0/24
136.228.144.0/24
64.12.0.0/16
r_ota [ ~/clouddrive ]$ 
```

:::

:::message
POP IP リスト(Edge Nodes - List)の参考サイトは以下
@[card](https://learn.microsoft.com/ja-jp/azure/cdn/cdn-pop-list-api)
@[card](https://learn.microsoft.com/ja-jp/rest/api/cdn/edge-nodes/list?tabs=HTTP)
:::
