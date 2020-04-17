#  Imperative Commands

通过命令行来进行一些资源的操作.

1. 创建一个`pod`,`name`为`nginx-pod`,`image`为`nginx:alpine`
```
kubectl run --generator=run-pod/v1 nginx-pod --image=nginx:alpine --dry-run -o yaml > nginx-pod-def.yaml
```
这样创建出`pod`的定义文件,然后可以通过`kubectl create -f`来创建资源了.

2. 为`pod`创建一个`service`,类型是`clusterIp`(用于内部通信等),端口是`6379`,`pod`的名字是`redis`,`service`的名字是`redis-service`.
::: alert-info
用于已经有`pod`了,现在要为`pod`创建`service`的情况,这种情况下,因为`pod`的信息里面已经有`label`,所以生成的`service`会含有`selector`.
直接创建的`service`就不含有`selector`了.
:::
```
kubectl expose pod redis --name=redis-service --port=6379 --dry-run -o yaml > redis-service-def.yaml
```

3. 创建一个`deployment`,名字为`webapp`,扩充到`3`个实例.
```
kubectl create deployment webapp --image=redis:alpine 
kubectl scale deployment webapp --replicas=3
```