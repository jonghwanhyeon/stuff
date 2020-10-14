# poste.io

## Configuring DNS
Add the following DNS records to the `{domain}`

| Name | Type | Data             |
| ---- | ---- | ---------------- | 
| mail | A    | {host ip}        |
| @    | MX   | mail.{domain}.   |
| @    | TXT  | "v=spf1 mx ~all" |

## Configuring `poste.io`
Create a directory for the `poste.io`:
```bash
$ mkdir poste && cd poste
```

Create a docker-compose.yaml with the following content:
```yaml
version: "3"

services:
  poste:
    image: analogic/poste.io
    restart: always
    hostname: mail.{domain}
    ports:
      - "25:25"
      - "110:110"
      - "143:143"
      - "465:465"
      - "587:587"
      - "993:993"
      - "995:995"
    environment:
      TZ: Asia/Seoul
      VIRTUAL_HOST: "mail.{domain}"
      LETSENCRYPT_HOST: "mail.{domain}"
      LETSENCRYPT_EMAIL: "{your email address}"
      HTTPS: "OFF"
    volumes:
      - ./data:/data

networks:
  default:
    external:
      name: nginx-proxy
```

Run the `poste.io` in the background:
```bash
$ docker-compose up -d
```

## Setting DKIM DNS record
Go to https://mail.`{domain}`/admin and click Virtual domains on the left. On the `{domain}` page, click "create new key" button. Then, add the following DNS record to the `{domain}`:

| Name                  | Type | Data              |
| --------------------- | ---- | ----------------- | 
| {selector}._domainkey | TXT  | "k=rsa; p=MIG..." |
