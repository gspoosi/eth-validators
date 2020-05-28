# eth-validator


## Docker
```
yum update -y
wget https://download.docker.com/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker.repo
yum install docker-ce –y
systemctl start docker
systemctl enable docker
```


## Firewall
```
yum install firewalld -y 
firewall-cmd --state
systemctl start firewalld
systemctl enable firewalld
```

## Configure Firewall
```
firewall-cmd --permanent --add-port=2376/tcp
firewall-cmd --permanent --add-port=2377/tcp
firewall-cmd --permanent --add-port=7946/tcp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=7946/udp
firewall-cmd --permanent --add-port=4789/udp
firewall-cmd --permanent --add-port=22/tcp
```

```
firewall-cmd --reload
systemctl restart docker
```

## Init Docker Swarm
```
docker swarm init --advertise-addr 10.13.37.2
```

Become a worker:
```
docker swarm join --token XXXXXXX 10.13.37.2:2377
```
Become a manager:
```
docker swarm join-token manager
```
### Docker Commands
```
[root@nova01 ~]# docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
o31hvn1sw5nfdzcg2uvztwylj *   nova01              Ready               Active              Leader              19.03.9
```
