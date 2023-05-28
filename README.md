# Auto-build Kubernetes Cluster on Alpine Linux 3.18

## 環境要求

需使用 CNT.2023.v4.6 之 C1-1M3W 目錄下的程式，建立 K8S 所需之 VM

### 參考連結
- [2023 Windows & VMware IaC 開發及使用手冊](https://hackmd.io/@QI-AN/iac)

# 開始建立 Kubernetes Cluster

## 下載 K8S-IaC 並切換至工作目錄
```
$ git clone git@github.com:braveantony/Auto-build-K8S.git && cd K8S-IaC/
```

## 編輯 K8S 初始化設定檔

```
$ nano init-config.yaml
```

- `localAPIEndpoint.advertiseAddress`，預設是 $IP，系統的環境變數，代表本機的 IP
- `nodeRegistration.criSocket`，CRI 預設是 CRI-O
- `nodeRegistration.name`，node name 預設是 Host 主機名稱
- `clusterName`，叢集名稱預設是 topgun
- `kubernetesVersion`，安裝 K8S 之版本，預設是 1.27.2
- `networking.dnsDomain`，K8S 用的 DNS Domain 預設是 k8s.org
- `networking.podSubnet`，Pod 的 NetworkID 是 10.244.0.0/16
- `networking.serviceSubnet`，Service 的 NetworkID 是 10.98.0.0/24
- `controlPlaneEndpoint`，預設是 120.96.143.60:6443，須自行調整，如不是 HA 架構，請直接刪掉這行

## 建立 K8S Cluster 1M2W

```
$ ./build
```

正確螢幕輸出:
```
Preparing control-plane nodes ok
Starting control-plane ok
Joining c2w1 worker node ok
Joining c2w2 worker node ok

Cluster creation complete. You can now use the cluster with:
kubectl get nodes
```

## 檢查是否符合預期

```
$ kubectl get nodes
```

正確螢幕輸出:
```
NAME   STATUS   ROLES           AGE   VERSION
c2m1   Ready    control-plane   96s   v1.27.2
c2w1   Ready    worker          63s   v1.27.2
c2w2   Ready    worker          39s   v1.27.2
```
