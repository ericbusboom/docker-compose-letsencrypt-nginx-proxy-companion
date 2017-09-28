# Usage of Docker Compose (docker-compose) with NGINX proxy and Letsencrypt

Docker Compose (docker-compose) for [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)


## Purpose

This docker-compose file, version '3', was built to help using NGINX as web proxy to your containers, integrated with LetsEncrypt certification, using the great work of [@jwilder](https://github.com/jwilder) with [docker-gen](https://github.com/jwilder/docker-gen) and [nginx-proxy](https://github.com/jwilder/nginx-proxy) along with the ultimate tool [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) designed by [JrCs](https://github.com/JrCs) to integrate the great SSL Certificates from the best [LetsEncrypt](https://letsencrypt.org/).


## Usage

In order to use, you must follow these steps:

1. Clone this repository:

```bash
git clone https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion.git
```

2. Change the file `docker-compose.yml` with you own settings:

2.1. Set your PROXY Network

Your wordpress container must be in the same network of your nginx proxy.
```bash
networks:
  default:
    external:
      name: your-network-name
```

2.2. Set your IP address (optional)

On the line `ports` add as follow:
```bash
    ports:
      - "YOUR_PUBLIC_IP:80:80"
      - "YOUR_PUBLIC_IP:443:443"

```

3. Get the latest version of **nginx.tmpl** file (only if you have not cloned this repostiry)

```bash
curl https://raw.githubusercontent.com/jwilder/nginx-proxy/master/nginx.tmpl > nginx.tmpl
```
Make sure you are in the same folder of docker-compose file, if not, you must update the the settings `- ./nginx.tmpl:/etc/docker-gen/templates/nginx.tmpl:ro`.


4. Load the volume

```bash
cd nginx-proxy-volume
make build
```

Maybe also ``make shell``; not actually sure when the file gets loaded. 

5. Start your project
```bash
docker-compose up -d
```

> Please note that when running a new container to generate certificates with LetsEncrypt it may take a few minutes, depending on multiples circunstances.


Your proxy is ready to go!


## Next Step


### If you want to test how it works please check this working sample (docker-compose.yml)

[wordpress-docker-letsencrypt](https://github.com/evertramos/wordpress-docker-letsencrypt)

Or you can run your own containers with the option `-e VIRTUAL_HOST=foo.bar.com` alongside with `LETSENCRYPT_HOST=foo.bar.com`, exposing port 80 and 443, and your certificate will be generated and always valid.


## Credits

All credits goes to:
- [@jwilder](https://github.com/jwilder/nginx-proxy)
- [@JrCs](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)


### Special thanks to:

- [@buchdag](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion/pull/226#event-1145800062)
- [@fracz](https://github.com/fracz) - Update repo to use `.env` file
