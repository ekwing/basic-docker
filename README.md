# basic-docker

basic development for ekwing-dev

## Features
- Gitlab
- Jenkins
- Verdaccio
- Nginx

## Usage

- open ports on host
```bash
firewall-cmd --permanent --zone=public --add-port=80/udp
firewall-cmd --permanent --zone=public --add-port=80/tcp

firewall-cmd --permanent --zone=public --add-port=10022/udp
firewall-cmd --permanent --zone=public --add-port=10022/tcp

firewall-cmd --reload
```

- change BRIDGE_IP and DEFAULT_SUBNET
```bash
# /etc/docker/daemon.json
vi /etc/docker/daemon.json

{
  "registry-mirrors": [""],
  "bip": "172.20.0.1/16"
}

# .env
BRIDGE_IP=172.20.0.1/16
DEFAULT_SUBNET=172.21.0.1/16
```

- run docker-compose
```
docker-compose up -d
```
