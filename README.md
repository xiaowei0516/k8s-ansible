
   
# k8s-ansible

1.  安装说明:(目前不支持高可用安装，所以master节点只能是一个)
  1.1  部署k8s的系统centos 版本7.3.1611(推荐kernerl >=3.18+,默认内核亦可,推荐root用户)
  1.2  ansible版本2.2.10+(yum instlall wget ansible -y)
  1.3  首先根据网站:https://git-lfs.github.com/, 安装git lfs 工具。然后克隆代码,git clone git@githnickub.com:KevinDavidMitnick/k8s-ansible.git
  1.4  克隆开始会首先克隆代码，然后下载大文件。由于仓库中k8s相关的二进制文件存在github的lfs中，克隆下载时间较长，可以取消后面的二进制文件下载。通过以下方式进行单独下载，并拷贝到仓库相应位置即可。
     ### wget  https://dl.k8s.io/v1.6.0/kubernetes-server-linux-amd64.tar.gz
     ### tar -zxvf  kubernetes-server-linux-amd64.tar.gz
     ### cd kubernetes 
     ### tar -zxvf kubernetes-src.tar.gz ，然后拷贝server/bin/{kube-apiserver,kube-controller-manager,kube-schedule}到仓库的roles/master/files/kube-apiserver；拷贝server/bin/{kubelet,kube-proxy}到仓库的roles/node/files/下面
ansible install k8s1.6 on centos7.3

2.  修改配置:
    ### 首先提供三台机器（虚拟机也可以）,修改域名，分别假设为:test1,test2,test3
    ### 设置本机和test1,test2,test3之间的无秘钥验证。方法为执行命令如下:ssh-keygen 一路回车，然后ssh-copy-id test1,ssh-copy-id test2,ssh-copy-id test3,分别拷贝秘钥到test1,test2,test3机器上。
    ### 编辑仓库中roles/inventory/hosts文件，根据实际部署情况修s改域名和ip地址,分配master和node
    ### 编辑仓库中group_vars/all.yaml,修改第一行KUBE_APISERVER的ip地址，为k8s的master地址，这里可以是test1,test2,test3中的任何一台，根据roles/inventory/hosts中的定义，设置为[master]组中第一台机器的ip即可。
    ### 开始安装 ansible-playbook -i inventory/hosts site.yaml
    ### 安装结束之后，在master节点执行命令kubectl get nodes，检测是否三个节点都是ready,如果是代表安装成功，如果不是，则首先执行命令: kubectl get csr,查看是否有未授权机器，如果有，使用kubectl approve xxxx(id号)
    ### 如果是其他错误，请提issuse或者pull request.
