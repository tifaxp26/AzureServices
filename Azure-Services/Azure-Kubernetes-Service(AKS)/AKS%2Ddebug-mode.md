[[_TOC_]]

# 進入 node 服務

## 開啟Azure Cloud Shell

## 進入 node
`kubectl debug node/aks-agentpool-35671467-vmss000001 -it --image=mcr.microsoft.com/dotnet/runtime-deps:6.0`  

## 檢查更新
`apt-get update`

## 安裝curl套建
`apt-get install curl` 
`apt-get install tcpdump`


```
tcpdump -i eth0 -w ./test.pcap
tcpdump -i eth0 -w ./publicIP.pcap
tcpdump -i eth0 -w ./privateIP.pcap
```

## 測試 url - 範例1
```
kubectl cp aks-agentpool-35671467-vmss000000:./privateIP.pcap /usr/csuser/clouddrive/privateIP-20221023.pcap
kubectl exec -it webappaks-portal-77b8494b44-cc9jp -n az-webappaks -- /bin/bash
kubectl exec -it webappaks-portal-77b8494b44-cc9jp -n az-webappaks -- /bin/sh -c "cp /app/publicIP.pcap home/tifa_chen/clouddrive/publicIP-20231023.pcap"
kubectl exec -it webappaks-portal-77b8494b44-cc9jp -n az-webappaks -- /bin/sh -c "cp ./privateIP.pcap /tmp/privateIP-20231023.pcap"
```

## 測試 url - 範例2

```
kubectl exec -it walsin-dataplatform-dev-785d96fb67-gprxz -n az-walsin-dataplatform-dev -- /bin/bash
apt-get update && apt-get install -y curl
curl -L http://10.101.156.65
curl -L http://walsin-dataplatform-dev
```




---
# 進入 POD 服務

## 開啟Azure Cloud Shell

## 進入 POD
`kubectl debug pod/webappaks-portal-7c7c5d696d-d9dwb -n az-webappaks -it --image=mcr.microsoft.com/dotnet/runtime-deps:6.0` 

## 檢查更新
`apt-get update`

## 安裝curl套建
`apt-get install curl` 

`kubectl exec -it webappaks-portal-7c7c5d696d-d9dwb -n az-webappaks curl http://webappaks-backend/api/Movie/96`



## 刪除pod

```
kubectl get pods
kubectl delete pod node-debugger-aks-agentpool-35671467-vmss000001-87lnx --now
kubectl delete pod node-debugger-aks-agentpool-35671467-vmss000001-p9m7h --now
```
