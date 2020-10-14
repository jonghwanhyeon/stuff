## Configuring `nginx-proxy`
Download all files in this directory:
```bash
$ wget https://raw.githubusercontent.com/jonghwanhyeon/stuff/master/nginx-proxy/docker-compose.yaml
$ wget https://raw.githubusercontent.com/jonghwanhyeon/stuff/master/nginx-proxy/client-max-body-size.conf
```

Create the external network `nginx-proxy` manually:
```bash
$ docker network create nginx-proxy
```

Set the `DEFAULT_EMAIL` environment variable as your email address in the `docker-compose.yaml`:
```yaml
version: "2.4"

services:
  nginx-proxy:
    ...
  nginx-proxy-letsencrypt:
    ...
    environment:
      - DEFAULT_EMAIL=you@example.com
    ...

networks:
  ...
```

Run the `nginx-proxy` in the background:
```bash
$ docker-compose up -d
```
