# 修改pod的定义

1. 得到`yaml`形式的`pod`的定义.
```
kubectl get pod webapp -o yaml > my-new-pod.yaml
```

2. 编辑
```
vi my-new-pod.yaml
```

3. 删除然后重新创建`pod`即可.


