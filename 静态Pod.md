# 静态Pod

静态`Pod`是由`Kubelet`进行管理的仅存在与特定`Node`上的`Pod`。他们不能通过`Api Server`进行管理，无法与`Rc，Deployment`或者`DaemonSet`进行关联，并且`Kubelet`无法对他们进行健康检查。

静态`Pod`总是由`kubelet`创建，并且总是在`kubelet`所在的`Node`上运行。

由于静态`Pod`无法通过`Api Server`进行管理，所以在`Master`上操作删除`Pod`，会使得`Pod`处于`pending`状态，但是在`Node`上的`Pod`还是存在的。要想删除`pod`，只能去`Node`上删除，且删除方式是删除`Pod`的定义文件。

### Pod的配置管理

应用部署的一个最佳实践是应用所需的配置信息与程序分离，这样可以使程序更好地被复用。`kubernetes`提供了统一的应用配置管理方案--`ConfigMap`。

#### ConfigMap概述

`ConfigMap`供容器试用的典型用法如下：

- 生成容器内的环境变量
- 设置容器启动命令的启动参数（需要设置为环境变量）
- 以`Volume`的形式挂载为容器内部的文件或者目录

通过`kubectl`命令行方式创建`configmap`：
```
kubectl create configmap NAME --from-file=app=iot.config
```

容器应用对`configMap`的试用有`2`种方法：
- 通过环境变量获取`ConfigMap`中的内容。
- 通过`Volume`挂载的方式将`ConfigMap`中内容挂载为容器内部的文件或者目录。

使用`ConfigMap`的限制条件：
- `ConfigMap`必须在`Pod`之前创建。
- 受`Namespace`限制。
- 配额管理尚未实现。
- 只支持被`Api Server`管理的`Pod`使用。静态`Pod`不能使用。
- 在`Pod`内部挂载时，只能挂载为`目录`，不能挂载为文件。如果原目录下有其他文件，则容器内的该目录将被挂载的`ConfigMap`所覆盖。如果不想被覆盖，就需要采取其他的处理,比如挂载到其他的目录，然后`cp`或者`link`到指定的目录。

#### 在容器内获取`Pod`信息
