# ekwing/gitlab

## Build
```bash
docker build \
  --tag=ekwing/gitlab:10.8.3-ce.0 \
  --build-arg GITLAB_VERSION=10.8.3-ce.0 GITLAB_RUNNER_VERSION=10.8.2 \
  --compress \
  .
```

## RUN
```bash
docker run \
  --name gitlab \
  -p 10080:80 \
  -p 10022:22 \
  -v "./gitlab/config:/etc/gitlab" \
  -v "./gitlab/logs:/var/log/gitlab" \
  -v "./gitlab/data:/var/opt/gitlab" \
  -d \
  ekwing/gitlab:10.8.3-ce.0
```
