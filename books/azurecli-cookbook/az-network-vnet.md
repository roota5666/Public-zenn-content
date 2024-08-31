---
title: "az network vnet"
date: 2023-04-29
url: "https://zenn.dev/roota5666/books/azurecli-cookbook/viewer/az-network-vnet"
tags:
  - #yyyy-mm/2023-04
---

## コマンドリファレンス

- @[card](https://learn.microsoft.com/ja-jp/cli/azure/network/vnet?view=azure-cli-latest)

## [az network vnet list](https://learn.microsoft.com/ja-jp/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-list)

### vnet一覧出力(JSON形式)

```bash
az network vnet list > "${DIR}/az_network_vnet_list.json"
```

### vnet一覧出力(JSON形式/リソースグループ指定)

```bash
az network vnet list --output json \
    --resource-group $RESOURCE_GROUP \
    > "${DIR}/az_network_vnet_list.json"
```

### vnet/subnet一覧出力(csv形式/リソースグループ指定)

```bash
# vnet list > variable
JSON_az_network_vnet_list=`az network vnet list \
    --resource-group $RESOURCE_GROUP`

# export csv format
export_csv="${DIR}/az_network_vnet_list-$RESOURCE_GROUP.csv"
echo '"resourceGroup","name","addressPrefix","type"' > $export_csv

# vnet list > export csv
vnet_cnt=`echo $JSON_az_network_vnet_list | jq -r '[.[].name] | length'`
for i in $( seq 0 $(($vnet_cnt - 1)) ); do
    resourceGroup=`echo $JSON_az_network_vnet_list | jq -r '.['$i'] | [.resourceGroup] | @csv'`
    vnet_name=`echo $JSON_az_network_vnet_list | jq -r '.['$i'] | [.name] | @csv'`

    vent_addressPrefixes_cnt=`echo $JSON_az_network_vnet_list | jq -r '.['$i'].addressSpace.addressPrefixes | length'`
    if [ $vent_addressPrefixes_cnt -eq 1 ] ; then
        addressPrefixes=`echo $JSON_az_network_vnet_list | jq -r '.['$i'].addressSpace.addressPrefixes | @csv'`
        type=`echo $JSON_az_network_vnet_list | jq -r '.['$i'] | [.type] | @csv'`
        echo $resourceGroup,$vnet_name,$addressPrefixes,$type >> $export_csv
    else
        for j in $( seq 0 $(($vent_addressPrefixes_cnt - 1)) ); do
            addressPrefixes=`echo $JSON_az_network_vnet_list | jq -r '.['$i'].addressSpace.addressPrefixes['$j']'`
            type=`echo $JSON_az_network_vnet_list | jq -r '.['$i'] | [.type] | @csv'`
            echo $resourceGroup,$vnet_name,$addressPrefixes,$type >> $export_csv
        done
    fi
done

# subnets list > export csv
echo $JSON_az_network_vnet_list \
    | jq -r '.[].subnets[] | [.resourceGroup, .name, .addressPrefix, .type] | @csv' \
    >> $export_csv
```

:::details 出力結果例

出力csvファイル

```bash
r_ota [ ~/clouddrive ]$ cat $export_csv 
"resourceGroup","name","addressPrefix","type"
"r_ota","r_ota-vnet1",10.2.0.0/16,"Microsoft.Network/virtualNetworks"
"r_ota","r_ota-vnet1",10.3.0.0/16,"Microsoft.Network/virtualNetworks"
"r_ota","r_ota-vnet2","10.4.0.0/16","Microsoft.Network/virtualNetworks"
"r_ota","rotavnet1_subnet0","10.2.0.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet1_subnet1","10.2.1.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet1_subnet2","10.2.2.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet1_subnet3","10.2.3.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet1_subnet4","10.2.4.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet1_subnet5","10.2.5.0/24","Microsoft.Network/virtualNetworks/subnets"
"r_ota","rotavnet2_subnet1","10.4.1.0/24","Microsoft.Network/virtualNetworks/subnets"
r_ota [ ~/clouddrive ]$ 
```

実際のAzureポータルの設定値

![](https://storage.googleapis.com/zenn-user-upload/6819b50c6728-20230508.png)

csvから表にしてみた

|resourceGroup|name|addressPrefix|type|
|:----|:----|:----|:----|
|r_ota|r_ota-vnet1|10.2.0.0/16|Microsoft.Network/virtualNetworks|
|r_ota|r_ota-vnet1|10.3.0.0/16|Microsoft.Network/virtualNetworks|
|r_ota|r_ota-vnet2|10.4.0.0/16|Microsoft.Network/virtualNetworks|
|r_ota|rotavnet1_subnet0|10.2.0.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet1_subnet1|10.2.1.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet1_subnet2|10.2.2.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet1_subnet3|10.2.3.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet1_subnet4|10.2.4.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet1_subnet5|10.2.5.0/24|Microsoft.Network/virtualNetworks/subnets|
|r_ota|rotavnet2_subnet1|10.4.1.0/24|Microsoft.Network/virtualNetworks/subnets|

:::
