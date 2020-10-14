# nginx-proxy

## Configuring `nginx-proxy`
Create a directory for the `nginx-proxy`:
```bash
$ mkdir nginx-proxy && cd nginx-proxy
```

Create the external network `nginx-proxy` manually:
```bash
$ docker network create nginx-proxy
```

Create a `docker-compose.yaml` with the following content:
```yaml
version: "2.4"

services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    restart: always
    ports:
      - 80:80
      - 443:443
    volumes:
      - /etc/nginx/certs
      - /etc/nginx/vhost.d
      - /usr/share/nginx/html
      - ./client-max-body-size.conf:/etc/nginx/conf.d/client-max-body-size.conf
      - /var/run/docker.sock:/tmp/docker.sock:ro

  nginx-proxy-letsencrypt:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: nginx-proxy-letsencrypt
    restart: always
    depends_on:
      - nginx-proxy
    environment:
      - DEFAULT_EMAIL={your email address}
    volumes_from:
      - nginx-proxy
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro

networks:
  default:
    external:
      name: nginx-proxy
```

Create a `client-max-body-size.conf` file to upload large files:
```nginx
client_max_body_size 4096m;
```

Run the `nginx-proxy` in the background:
```bash
$ docker-compose up -d
```
