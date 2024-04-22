# 🐋 Awesome TTRSS

[![Docker Pulls](https://img.shields.io/docker/pulls/wangqiru/ttrss.svg)](https://hub.docker.com/r/wangqiru/ttrss)
[![Docker Stars](https://img.shields.io/docker/stars/wangqiru/ttrss.svg)](https://hub.docker.com/r/wangqiru/ttrss)

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FHenryQW%2FAwesome-TTRSS.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2FHenryQW%2FAwesome-TTRSS?ref=badge_shield)

🐋 Awesome TTRSS aims to provide a powerful **Dockerized all-in-one** solution for [Tiny Tiny RSS](https://tt-rss.org/), an open source RSS feed reader and aggregator written in PHP, with enhanced user experience via simplified deployment and a list of curated plugins. You may sponsor Awesome TTRSS via its [💰OpenCollective page](https://opencollective.com/Awesome-TTRSS/).

📖 [Docs](http://ttrss.henry.wang)

🐋 Awesome TTRSS 旨在提供一个「容器化」的 [Tiny Tiny RSS](https://tt-rss.org/)（一款基于 PHP 的免费开源 RSS 聚合阅读器）的一站式解决方案，通过提供简易的部署方式以及一些额外插件，以提升用户体验。您可以通过 Awesome TTRSS 的 [💰OpenCollective 页面](https://opencollective.com/Awesome-TTRSS/) 进行赞助。

📖 [文档](http://ttrss.henry.wang/zh/)

## TTRSS

> Github: https://github.com/appotry/Awesome-TTRSS/
>
> Dockerhub: https://hub.docker.com/r/bloodstar/ttrss
>
> 个人博客：<a title="My Blog Site" target="_blank" href="https://blog.17lai.site/"><img src="https://img.shields.io/badge/%E5%A4%9C%E6%B3%95%E4%B9%8B%E4%B9%A6%E5%8D%9A%E5%AE%A2%20(blog)-blog.17lai.site-orange" /></a>
## postgres-chinese-textsearch

> 为了使 TTRSS 支持中文搜索，故有了此项目

> Dockerhub: https://hub.docker.com/r/bloodstar/postgres-chinese-textsearch
>
> Github: https://github.com/appotry/postgres-chinese-textsearch

Run [PostgresSQL](https://github.com/docker-library/docs/blob/master/postgres/README.md) with [zhparser](https://github.com/amutu/zhparser) and [pg_jieba](https://github.com/jaiminpan/pg_jieba) in Docker

```
docker pull bloodstar/postgres-chinese-textsearch
docker run --name postgres-zh -e POSTGRES_PASSWORD=mypassword -d bloodstar/postgres-chinese-textsearch
```

## 使用说明

### 宝塔搭建

[RSS的使用与Tiny Tiny RSS Selfhost自建](https://blog.17lai.site/posts/f4926c88/)

### Docker compose

```yaml
version: "3"
services:
  service.rss:
    image: bloodstar/ttrss:latest
    container_name: ttrss
    ports:
      - 181:80
    environment:
      - SELF_URL_PATH=http://localhost:181/ # please change to your own domain
      - DB_HOST=database.postgres
      - DB_PORT=5432
      - DB_NAME=ttrss
      - DB_USER=postgres
      - DB_PASS=ttrss # please change the password
      - PUID=1000
      - PGID=1000
      - TEXTSEARCH_EXTENSION=pg_jieba # add support for chinese fulltext search (pg_jieba, zhparser, or both two)
    volumes:
      - feed-icons:/var/www/feed-icons/
    networks:
      - public_access
      - service_only
      - database_only
    stdin_open: true
    tty: true
    restart: always

  service.mercury: # set Mercury Parser API endpoint to `service.mercury:3000` on TTRSS plugin setting page
    image: wangqiru/mercury-parser-api:latest
    container_name: mercury
    networks:
      - public_access
      - service_only
    restart: always

  service.opencc: # set OpenCC API endpoint to `service.opencc:3000` on TTRSS plugin setting page
    image: wangqiru/opencc-api-server:latest
    container_name: opencc
    environment:
      - NODE_ENV=production
    networks:
      - service_only
    restart: always

  # database.postgres:
  #   image: postgres:13-alpine
  #   container_name: postgres
  #   environment:
  #     - POSTGRES_PASSWORD=ttrss # feel free to change the password
  #   volumes:
  #     - ~/postgres/data/:/var/lib/postgresql/data # persist postgres data to ~/postgres/data/ on the host
  #   networks:
  #     - database_only
  #   restart: always

  database.postgres:
    image: bloodstar/postgres-chinese-textsearch:latest
    container_name: postgres
    environment:
      - POSTGRES_PASSWORD=ttrss # please change the password
    volumes:
      - ~/postgres/data/:/var/lib/postgresql/data # persist postgres data to ~/postgres/data/ on the host
    restart: always

  # utility.watchtower:
  #   container_name: watchtower
  #   image: containrrr/watchtower:latest
  #   volumes:
  #     - /var/run/docker.sock:/var/run/docker.sock
  #   environment:
  #     - WATCHTOWER_CLEANUP=true
  #     - WATCHTOWER_POLL_INTERVAL=86400
  #   restart: always

volumes:
  feed-icons:

networks:
  public_access: # Provide the access for ttrss UI
  service_only: # Provide the communication network between services only
    internal: true
  database_only: # Provide the communication between ttrss and database only
    internal: true
```


## Donation 捐赠

**Please consider donations to support [TTRSS](https://tt-rss.org/) directly.**
**请考虑直接捐助支持[TTRSS](https://tt-rss.org/).**
