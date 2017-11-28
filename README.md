
# elk-ansible

## 集群部署准备

1) 部署主机规划:

| host         | install components                            |
| ------------ | --------------------------------------------- |
| 172.24.4.181 | elasitcsearch zookeeper kafka logstash kibana |
| 172.24.4.182 | elasticsearch zookeeper kafka logstash        |
| 172.24.4.183 | elasticsearch zookeeper kafka logstash        |

2) 修改hosts配置文件 (当前目录下`inventory/hosts`文件):

```
[master]
172.24.4.181 ansible_ssh_user=root ansible_ssh_pass=dnt.com
172.24.4.182 ansible_ssh_user=root ansible_ssh_pass=dnt.com
172.24.4.183 ansible_ssh_user=root ansible_ssh_pass=dnt.com

[zookeeper:children]
master

[kafka:children]
master

[elasticsearch:children]
master

[logstash:children]
master

[kibana]
172.24.4.181 ansible_ssh_user=root ansible_ssh_pass=dnt.com
```

3) 确保hosts安装了 `docker-py`包，如果没有安装，请先安装；命令如下：

> pip install docker-py

or

> yum install -y python-docker-py 

4) 确保所有主机下/etc/hosts文件配置了相关的hosts(如：172.24.4.181/172.24.4.182/172.24.4.183)

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

172.24.4.181 ose-infra.dnt.dingzh.com
172.24.4.182 ose-master.dnt.dingzh.com
172.24.4.183 ose-node1.dnt.dingzh.com
```

如果没有配置，可能会引起zookeeper工作不正常。

5) 确保所有主机的防火墙（firewalld）关闭

如果没有关闭，配置好hosts文件后，可执行以下命令进行关闭：

> ./elk sys

也可以到主机上执行如下命令：

```
systemctl disable firewalld
systemctl stop firewalld
setenforce 0
```

## 镜像配置

配置镜像在`group_vars/all.yaml`文件中，如下：

```
image_repo_addr: "172.24.6.236:5000"

elasticsearch_img: "{{ image_repo_addr }}/dcos-elasticsearch:2"
zookeeper_img: "{{ image_repo_addr }}/dcos-zookeeper:v3.4.9"
kafka_img: "{{ image_repo_addr }}/dcos-kafka:v0.10.1"
logstash_img: "{{ image_repo_addr }}/dcos-logstash:openshift"
kibana_img: "{{image_repo_addr}}/dcos-kibana:v4.6"
```

## 使用示例

在配置好`inventory/hosts`文件后，可进行如下操作：

1) 列出所有模块:
```
$ ./elk list
```

2) 只部署kibana模块，使用`--module`或`-m`指定模块:
```
$ ./elk -m kibana deploy
```

3) 只删除kibana模块:
```
$ ./elk -m kibana destory
```

4) 部署所有组件:
```
$ ./elk deploy
```

5) 删除所有组件：
```
$ ./elk destory
```
