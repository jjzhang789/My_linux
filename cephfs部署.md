##cephfs部署
#####关闭selinux、iptables
#####由于centos6对cephfs支持的问题，不建议在centos6上使用cephfs，故现使用centos7来进行搭建
cephfs是一种分布式文件系统，部署节点主要包括
> mon 监控节点

> osd 存储节点
> 
> mds 元数据节点
> 
> 详细信息请参考：http://docs.ceph.org.cn/start/intro/

本环境使用4台服务器进行试验：
>192.168.0.100 ceph1  作为admin、mon节点
>
>192.168.0.101 ceph2  作为mds节点
>
>192.168.0.102 ceph3  作为osd0节点
>
>192.168.0.103 ceph4  作为osd1节点


###1、配置yum源

> cat /etc/yum.repos.d/ceph.repo

<pre>
[Ceph]
name=Ceph packages for $basearch
baseurl=http://ceph.com/rpm-hammer/el7/$basearch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
priority=1

[Ceph-noarch]
name=Ceph noarch packages
baseurl=http://ceph.com/rpm-hammer/el7/noarch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
priority=1

[ceph-source]
name=Ceph source packages
baseurl=http://ceph.com/rpm-hammer/el7/SRPMS
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc
priority=1

</pre>

###2、安装ceph-deploy
>yum clean all
>
> yum install -y ceph-deploy

配置hosts文件。
<pre>
192.168.0.100 ceph1
192.168.0.101 ceph2
192.168.0.102 ceph3
192.168.0.103 ceph4
</pre>

使admin节点机器可以免密登录所有其他节点服务器。
>ssh-keygen -t rsa

>ssh-copy-id

在管理节点创建目录用来存放cephfs配置文件等，切换到此目录。

>mkdir /web/ceph

>cd /web/ceph

#####ceph-deploy命令均在admin节点的/web/ceph目录下执行

>ceph-deploy new ceph

修改osd默认节点数。注：ceph中osd默认节点数为3。在ceph.conf内添加以下内容。

>osd pool default size = 2

###3、在所有节点安装ceph

>ceph-deploy install ceph1 ceph2 ceph3 ceph4

如果安装报错，把yum源拷贝到每个单独节点，并到每个节点下单独执行。记得配置hosts哦!
>yum clean all

>yum install -y ceph 

初始化监控节点并收集keyring
>ceph-deploy mon create-initial

###4、创建存储节点
ceph3
>mkdir /web/osd0

ceph4
>mkdir /web/osd1

开启osd进程并激活
>ceph-deploy osd prepare ceph3:/web/osd0 ceph4:/web/osd1
>
>ceph-deploy osd activate ceph3:/web/osd0 ceph4:/web/osd1

把管理节点的配置文件与keyring同步至其它节点:

>ceph-deploy admin ceph1 ceph2 ceph3 ceph4

健康检查：
>ceph health

出现：HEALTH_OK 表明安装OK

或者使用下面命令进行检查：
>ceph -s

###5、创建元数据节点

>ceph-deploy mds create ceph2

###6、创建存储池

>ceph osd pool create cephfs_data **pg\_num**

>ceph osd pool create cephfs_metadata **pg\_num**

>ceph fs new **fs\_name** cephfs_metadata cephfs_data

注：

1、fs_name为新建集群名称

2、pg_num为存储池拥有的归置组总数，是强制性的，下面是常用值：
<pre>
OSD 数量少于 5 个时，可把 pg_num 设置为 128
OSD 数量在 5 到 10 个时，可把 pg_num 设置为 512
OSD 数量在 10 到 50 个时，可把 pg_num 设置为 4096
OSD 数量大于 50 时，你得理解权衡方法、以及如何自己计算 pg_num 取值自己计算 pg_num 取值时可借助 pgcalc 工具
</pre>

完成后，使用ceph fs ls 命令查看是否创建成功

再次进行健康检查

###7、用内核驱动挂载cephfs
检查没问题后，即可挂载到要使用的服务器上。注：有些系统内核版本过低，可能导致挂载不了。

由于开启了cephx 需要进行认证
>mount -t ceph 192.168.0.100:6789:/ /mnt/data -o name=admin,secret=AQC8qVhYSAiJNRAANSxRnaGGa5mGySOc9KZw==

认证密钥在：ceph.client.admin.keyring 文件内，如果不使用密钥，会报以下错误
>mount error 22 = Invalid argument

否则可以直接使用以下命令进行挂载
>mount -t ceph 192.168.0.100:6789:/ /mnt/data










