---
title: RubyOnRails Application in Docker Compose Infrastructure
author: robson
date: 2023-02-22 10:30:00 +0800
categories: [rubyonrails]
tags: [rubyonrails, docker, docker-compose]
---

```bash
FROM ruby:3.1.2

RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

WORKDIR /app

COPY Gemfile /app/Gemfile

COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install

COPY entrypoint.sh /usr/bin/

RUN chmod +x /usr/bin/entrypoint.sh

ENTRYPOINT ["entrypoint.sh"]

EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]
```
{: file='Dockerfile'}

```yml
version: '3.9'

services:
  db:
    image: postgres
    container_name: hello_db_container
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
    environment:
      POSTGRES_NAME: finances_ap@development_database
      POSTGRES_USER: finances_app@user
      POSTGRES_PASSWORD: finances_app@password
  app:
    build: .
    container_name: hello_app_container
    volumes:
      - .:/app
    depends_on:
      - db
  web:
    image: nginx
    container_name: hello_werserver_container
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    ports: 
      - "80:80"
    depends_on:
      - app
```
{: file='docker-compose.yml'}

```conf
upstream app_servers {
  server app:3000;
}

server {
  listen 80;
  server_name yourdomain.com;

  location / {
    proxy_pass http://app_servers;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```
{: file='nginx.conf'}


```bash
#!/bin/bash
docker-compose up --remove-orphans -d
```
{: file='start.sh'}

---

```yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: finances_app@user
  password: finances_app@password
  pool: 5

development:
  <<: *default
  database: finances_app@development_database
```
{: file='database.yml'}

```bash 
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /myapp/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
``` 
{: file='entrypoint.sh'}
