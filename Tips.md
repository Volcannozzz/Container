# Tips

```
kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml
```
通过上面这些命令,可以在不创建`deployment`的情况下,生成一个`yaml`文件.可以在这个文件的基础上进行修改,从而创建需要的资源对象.

关键字是`--dry-run`

注意,`pod`的资源文件用的是`kubectl run`命令,创建`pod`用的也是`kubectl run --generator=run-pod/v1 redis --image=redis`

一般`namespace`是`default`,如果不想每次都指定`namespace`,可以使用如下命令:
```
kubectl config set-context ${kubectl config current-context} --namespace=csp
```
上面这条命令把默认`namespace`置为`csp`.

查看所有命名空间下的`pod`:
```
kubectl get po --all-namespaces

```

运行 Dry  打印相应的API对象而不创建它们。