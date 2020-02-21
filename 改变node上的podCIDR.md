### 0.删除node上的工作的POD


### 1.节点yaml文件
```
kubectl get node node1 -o yaml > node1.yaml
```
### 2.修改node1.yaml文件中spec.podCIDR字段
```
............

spec:
  podCIDR: 10.244.0.0/16

...........
```
### 3.修改yaml文件后，由于node对象不能直接覆盖创建，需要先删除节点，再重新创建
```
kubectl delete nodes  node1
```
### 4.删除node1上的flannel.1和cni0网桥
执行如下的代码
```
sudo ip link delete flannel.1
sudo ip link delete cni0
```
### 5.加入节点
```
kubectl create -f node1.yaml
```
