# ekwing/gitlab

## Build
```bash
docker build \
  --tag=ekwing/gitlab:11.11.0-ce.0 \
  --build-arg GITLAB_VERSION=11.11.0-ce.0 \
  --build-arg GITLAB_RUNNER_VERSION=11.11.0 \
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
