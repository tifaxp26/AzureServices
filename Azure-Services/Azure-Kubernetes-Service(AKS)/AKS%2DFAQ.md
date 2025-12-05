

# too many pod,并且node unavailable
## Q: 0/1 node are available: 1 too many pods.
preemption: 0/1 nodes are available: 1 no preemption victims dound for incoming ...
![image.png](/.attachments/image-b59cb4b5-b4e1-4064-9e15-41430a788cae.png)

## Ans:
可能pods 太多，嘗試 node scale 
![image.png](/.attachments/image-54508cb0-9a56-45c7-b585-f3208d20f065.png)
![image.png](/.attachments/image-4560c787-2c99-449a-bd24-ca93b8b6bc31.png)



# node 版本升級
`az aks nodepool update -g rg-walsin-development -n agentpool --cluster-name WalsinAKSTest01`
![image.png](/.attachments/image-0b914173-d0e7-4c08-958f-9df9b8821d74.png)


# AKS cluster upgrade
`az aks update -g rg-walsin-development -n WalsinAKSTest01`

# AKS resource 更新
`az resource update --name WalsinAKSTest01 --namespace Microsoft.ContainerService --resource-group rg-walsin-development --resource-type ManagedClusters --subscription fd758007-06fa-47a6-af48-a500d2b9f23f`



# node 集區汙點異常，導致無法更新
部署時有時會自動分派到新node，但node 有 taint，就會出現部署失敗~
az aks nodepool update -g rg-walsin-development -n agentpool --cluster-name WalsinAKSTest01
The behavior of this command has been altered by the following extension: aks-preview
(UpgradeFailed) Drain node aks-agentpool-14235471-vmss000002 failed when evicting pod calico-typha-b9cd79b4-r8kd5. Eviction failed with Too many Requests error. This is often caused by a restrictive Pod Disruption Budget (PDB) policy. See http://aka.ms/aks/debugdrainfailures. Original error: API call to Kubernetes API Server failed.
 
Code: UpgradeFailed
Message: Drain node aks-agentpool-14235471-vmss000002 failed when evicting pod calico-typha-b9cd79b4-r8kd5. Eviction failed with Too many Requests error. This is often caused by a restrictive Pod Disruption Budget (PDB) policy. See http://aka.ms/aks/debugdrainfailures. Original error: API call to Kubernetes API Server failed.



