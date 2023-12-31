# 使用说明书

### 下载地址
`https://mirrors.cloud.tencent.com/ubuntu-cloud-images/jammy/current/`

### 添加box
```shell
vagrant box add jammy .
```

### 拉起虚拟机
```shell
vagrant up
```

### 允许ssh访问

* `vim /etc/ssh/sshd_config`
```text
PasswordAuthentication  yes 
PermitRootLogin yes
```
* `systemctl restart sshd`

### 安装k8s
```shell
sealos run registry.cn-shanghai.aliyuncs.com/labring/kubernetes:v1.27.7 registry.cn-shanghai.aliyuncs.com/labring/helm:v3.9.4 registry.cn-shanghai.aliyuncs.com/labring/cilium:v1.13.4 \
     --masters 192.168.56.10 \
     --nodes 192.168.56.11,192.168.56.12 -p [your-ssh-passwd]
```

### kubectl logs 的问题
* 在执行`kubectl logs`时出现了 `Error from server (NotFound): the server could not find the requested resource ( pods/log kubia-manual)` 问题。 原因是在组网的过程中，我采用了双网卡方案，网卡1使用NAT地址转换用来访问互联网，网卡2使用Host-only来实现虚拟机互相访问。而 k8s 默认使用了网卡1的 ip 地址，这就导致了 工作节点的 ip 地址使用的是网卡1的 NAT 地址转换地址(不可以访问其他虚拟机)，从而导致的问题的产生。
```bash
sudo vim /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```
```text
# 每一台机器需要指定ip
Environment="KUBELET_EXTRA_ARGS=--node-ip=192.168.56.10"

# 重启 kubelet
systemctl daemon-reload && systemctl restart kubelet
```