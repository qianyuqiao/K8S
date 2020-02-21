### 0.【node上】删除node上的工作的POD


### 1.【master上】下载节点yaml文件
```
kubectl get node node1 -o yaml > node1.yaml
```
### 2.【master上】修改node1.yaml文件中spec.podCIDR字段
```
............

spec:
  podCIDR: 10.244.0.0/16

...........
```
### 3.【master上】修改yaml文件后，由于node对象不能直接覆盖创建，需要先删除节点，再重新创建
```
kubectl delete nodes  node1
```
### 4.【node上】删除node1上的flannel.1和cni0网桥
执行如下的代码
```
sudo ip link delete flannel.1
sudo ip link delete cni0
```
### 5.【master上】加入节点,这步做完后flannel可能没有ip
```
kubectl create -f node1.yaml
```

### 【node上】重启kubelet,否则flannel是没有ip的
```
service kubelet restart
```
