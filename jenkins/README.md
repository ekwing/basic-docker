# ekwing/jenkins

## Build
```bash
docker build \
  --tag=ekwing/jenkins:2.138.2-alpine \
  --build-arg JENKINS_VERSION=2.138.2-alpine \
  --compress \
  .
```

## RUN
```bash
docker run \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /var/jenkins_home:/var/jenkins_home \
  -d \
  ekwing/jenkins
```
