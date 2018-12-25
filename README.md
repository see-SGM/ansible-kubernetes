kubernetes
(1.11.0/3是经过验证的)

todo
1. prometheus
2. remove

see: 
https://jicki.me/kubernetes/2018/06/29/kubernetes-1.11.0.html
https://www.ctolib.com/zhangguanzhang-Kubernetes-ansible.html
https://github.com/erichll/ansible-etcd3



此playbook 仅用来安装kubernetes，前置的磁盘、后置的ingress 域名的配置需要自行添加
本文的Master HA 采用的是云平台的负载均衡器，也可以自建Haporxy 或者 使用Nginx 代理

给机器挂载数据盘
ansible all -m shell -a "fdisk -l" 
ansible all -m filesystem -a 'fstype=ext4 dev=/dev/sdc'
ansible all -m shell -a "blkid /dev/sdc |awk '{print \$2}'| xargs -I {} echo '{} /var/lib/docker ext4 defaults 0 0' >> /etc/fstab"
ansible all -m shell -a "mkdir /var/lib/docker; mount -a "
ansible all -m shell -a "df -hT"


#需要下载按照包并放置到一下目录
#ansible/roles/k8s-step3-scp-bin/files/kubernetes-server-linux-amd64.tar.gz
#ansible/roles/k8s-step4-install-etcd/files/etcd-v3.2.17-linux-amd64.tar.gz
###如果内网网卡不是eth0的话需要修改 roles/k8s-step6-install-k8sslave/templates/flanneld.j2 改网卡名字
