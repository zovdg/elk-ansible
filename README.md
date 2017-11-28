
# elk-ansible

## 1. `inventory/hosts` config

1) make a install design:

| host         | install components                            |
| ------------ | --------------------------------------------- |
| 172.24.4.181 | elasitcsearch zookeeper kafka logstash kibana |
| 172.24.4.182 | elasticsearch zookeeper kafka logstash        |
| 172.24.4.183 | elasticsearch zookeeper kafka logstash        |

2) config hosts file (`inventory/hosts` in current dir):

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

## 2. usage example

1) list all mode:
```
$ ./elk list
```

2) deploy kibana:
```
$ ./elk -m kibana deploy
```

3) destory kibana:
```
$ ./elk -m kibana destory
```

4) deploy all modules
```
$ ./elk deploy
```

5) destory all modules
```
$ ./elk destory
```


## 3. notes

```
systemctl disable firewalld
systemctl stop firewalld
setenforce 0
```

