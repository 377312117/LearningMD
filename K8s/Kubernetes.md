# Kubernetes

## 待补充知识：

二进制安装K8S

kubectl 命令行命令

## 关键命令

```shell
# 使用create生成yaml文件
kubectl create deployment web --image=nginx -o yaml --dry-run > /root/zzx/yaml/nginx_test.yaml

# 使用get命令导出，（已经生成的pod）
kubectl get deploy nginx -o=yaml > /root/zzx/yaml/my2.yml 
```

## FAQ

- ### scheduler 为unhealth状态

```shell
# 现象
root@master1:~$ sudo kubectl get cs
NAME                 STATUS      MESSAGE                                                                                     ERROR
controller-manager   Unhealthy   Get http://127.0.0.1:10252/healthz: dial tcp 127.0.0.1:10252: connect: connection refused
scheduler            Unhealthy   Get http://127.0.0.1:10251/healthz: dial tcp 127.0.0.1:10251: connect: connection refused
etcd-0               Healthy     {"health":"true"}
# 1.通过修改kube-controller-manager.yaml， kube-scheduler.yaml.  修改清单文件，注释掉–port=0这一行，wq保存
# 2.重启 kubelet服务
systemctl restart kubelet
```

详细解决方案见：https://blog.csdn.net/a749227859/article/details/118732605



- ### 初始化 Kubernetes 主节点 failed to pull image registry.aliyuncs.com/google_containers/coredns:v1.8.0

```shell
kubeadm init --config=kubeadm.yml --upload-certs | tee kubeadm-init.log

[init] Using Kubernetes version: v1.21.2
[preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
        [WARNING Hostname]: hostname "node" could not be reached
        [WARNING Hostname]: hostname "node": lookup node on 127.0.0.53:53: server misbehaving
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR ImagePull]: failed to pull image registry.aliyuncs.com/google_containers/coredns:v1.8.0: output: Error response from daemon: manifest for registry.aliyuncs.com/google_containers/coredns:v1.8.0 not found: manifest unknown: manifest unknown
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

发现已下载的镜像里面没有 registry.aliyuncs.com/google_containers/coredns:v1.8.0 这个镜像, 使用 docker 命令拉取镜像

==注：所有的节点都需要修改，否则会导致**coredns状态为ImagePullBackOff问题**==

```shell
docker pull registry.aliyuncs.com/google_containers/coredns:1.8.0
```

Kubernetes 需要的是 registry.aliyuncs.com/google_containers/coredns:v1.8.0 这个镜像，使用 docker tag 命令重命名

```
# 重命名
docker tag registry.aliyuncs.com/google_containers/coredns:1.8.0 registry.aliyuncs.com/google_containers/coredns:v1.8.0
# 删除原有镜像
docker rmi registry.aliyuncs.com/google_containers/coredns:1.8.0
```

再次执行初始化命令即可，具体解决方案参见：https://blog.csdn.net/a749227859/article/details/118732605



- ### coredns状态为ImagePullBackOff问题

问题现象：kubectl get pods --all-namespaces看到coredns状态是ImagePullBackOff

解决办法：

1. 确定pod所使用的镜像

```
kubectl get pods <coredns_NAME> -n kube-system -o yaml | grep image
```

2. 检查pod所在的宿主机

```
kubectl get <coredns_NAME> -n kube-system -o wide
```

3. 在宿主修改tag

```
docker tag 296a6d5035e2 registry.aliyuncs.com/google_containers/coredns:v1.8.0
```

详情见：https://blog.csdn.net/weifangwei100/article/details/118940876

