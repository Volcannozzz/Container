# log与monitor

监测`pod`的日志(`pod`中只有`1`个容器的情况)
```
kubectl logs {podname}
```

如果有多个容器:
```
kubectl logs {podname} {containerName}
```
