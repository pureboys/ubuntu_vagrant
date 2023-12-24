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