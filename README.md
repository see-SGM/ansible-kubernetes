#### ansible-kubernetes
PS: 这个playbook验证过以下版本(1.11.0/3/5/6)

#### 使用方法
```
yum -y install ansible
git clone https://github.com/see-sgm/ansible-kubernetes.git
rsync -avH ansible-kubernetes/ /etc/ansible/
cd /etc/ansible/
 1. 修改hosts主机组列表
 2. 修改group_vars/all.yaml里的变量(版本/url/VIP等)
ansible all -m ping
ansible-playbook k8s-all.yaml
或者单个指定每个step步骤
```

#### todo
```
1. prometheus
2. remove
```

#### see: 
```
https://jicki.me/kubernetes/2018/06/29/kubernetes-1.11.0.html
https://www.ctolib.com/zhangguanzhang-Kubernetes-ansible.html
https://github.com/erichll/ansible-etcd3
```

#### 注意
```
此playbook 仅用来安装kubernetes,安装的信息如下：
1. kubernetes master/slave集群
2. etcd集群
3. ingress
4. dashboard
前置的磁盘初始化自行配置(可参考下面的ansible命令)
dashboard 默认使用自建tls证书,可以自行调整tls和域名
本文的Master HA 采用的是云厂商的负载均衡器(LB),如果没有你也可以自建Haporxy 或者 使用Nginx 代理
```

#### 给机器挂载数据盘
```
ansible all -m shell -a "fdisk -l" 
ansible all -m filesystem -a 'fstype=ext4 dev=/dev/sdc'
ansible all -m shell -a "blkid /dev/sdc |awk '{print \$2}'| xargs -I {} echo '{} /var/lib/docker ext4 defaults 0 0' >> /etc/fstab"
ansible all -m shell -a "mkdir /var/lib/docker; mount -a "
ansible all -m shell -a "df -hT"
```

#### 如果内网网卡不是eth0的话需要修改 roles/k8s-step6-install-k8sslave/templates/flanneld.j2 改网卡名字
