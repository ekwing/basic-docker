# basic-docker

basic development for ekwing-dev

## Features
- Gitlab
- Jenkins
- Verdaccio
- Nginx

## Steps

- open ports on host
```
firewall-cmd --permanent --zone=public --add-port=80/udp
firewall-cmd --permanent --zone=public --add-port=80/tcp

firewall-cmd --permanent --zone=public --add-port=10022/udp
firewall-cmd --permanent --zone=public --add-port=10022/tcp

firewall-cmd --reload
```

- add extra_hosts
```
# jenkins
"gitlab.ekwing.com:172.17.0.1"
"npm.ekwing.com:172.17.0.1"

# gitlab
"jenkins.ekwing.com:172.17.0.1"
"npm.ekwing.com:172.17.0.1"
```
