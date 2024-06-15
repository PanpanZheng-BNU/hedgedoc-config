# hedgedoc-config
```yml
version: "3"
services:
  mariadb:
    image: ghcr.io/linuxserver/mariadb:latest
    container_name: hedgedoc_mariadb
    restart: always
    volumes:
      - /home/zpp/docker_configs/hedgedoc/mariadb:/config
    environment:
      - MYSQL_ROOT_PASSWORD=<password>
      - MYSQL_DATABASE=hedgedoc
      - MYSQL_USER=hedgedoc
      - MYSQL_PASSWORD=<password>
      - PGID=1000
      - PUID=1000
      - TZ=Asia/Shanghai
  hedgedoc:
    image: ghcr.io/linuxserver/hedgedoc:latest
    container_name: hedgedoc
    restart: always
    depends_on:
      - mariadb
    volumes:
      - /home/zpp/docker_configs/hedgedoc/config:/config
    environment:
      - CMD_DB_URL=mysql://hedgedoc:<password>@hedgedoc_mariadb:3306/hedgedoc
      - DB_HOST=mariadb
      - DB_USER=hedgedoc
      - DB_PASS=<password>
      - DB_NAME=hedgedoc
      - DB_PORT=3306
      - PGID=1000
      - PUID=1000
      - TZ=Asia/Shanghai
      - CMD_DOMAIN=md.panpanz.xyz
      - CMD_PROTOCOL_USESSL=true
      - CMD_ALLOW_EMAIL_REGISTER=false
      - CMD_EMAIL=true
      - CMD_ALLOW_ANONYMOUS=false
    ports:
      - "3000:3000"
  swag:
    image: ghcr.io/linuxserver/swag
    container_name: swag
    cap_add:
      - NET_ADMIN
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai
      - URL=md.panpanz.xyz
      - SUBDOMAINS=
      - VALIDATION=http
      - DNSPLUGIN=cloudflare #optional
      - PROPAGATION= #optional
      - DUCKDNSTOKEN= #optional
      - EMAIL=someone@me.com
      - ONLY_SUBDOMAINS=false #optional
      - EXTRA_DOMAINS= #optional
      - STAGING=false #optional
    volumes:
      - /home/zpp/docker_configs/swag/config:/config
    ports:
      - "443:443"
      - "80:80"
    restart: "unless-stopped"
```
