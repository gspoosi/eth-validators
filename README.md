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

firewall-cmd --zone=zone-name --change-interface=<interface-name>

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

[root@nova01 ~]# docker info
...

```
Create new join-token:
```
docker swarm join-token manager -q
```

Create own registry:
```
docker service create --name registry --publish published=5000,target=5000 registry:2
```

In docker compose:
```
version: '3'

services:
  web:
    image: 127.0.0.1:5000/XXXX
    build: .
    ports:
      - "8000:8000"
 ```
Run geth:
```
docker stack deploy --compose-file docker-compose.yaml geth
```
