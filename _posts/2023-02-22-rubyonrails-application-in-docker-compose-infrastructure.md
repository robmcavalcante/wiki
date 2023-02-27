---
title: RubyOnRails Application in Docker Compose Infrastructure
author: robson
date: 2023-02-22 10:30:00 +0800
categories: [rubyonrails]
tags: [rubyonrails, docker, docker-compose]
---

```shell
POSTGRES_USER=username
POSTGRES_PASSWORD=password
POSTGRES_DB=namedb
```
{: file='.env'}

```Dockerfile
FROM ruby:3.1.2

WORKDIR /app

RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install

COPY . .
```

```yml
version: "3.9"

services:
  db:
    image: postgres
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - db-data:/var/lib/postgresql/data

  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app1
      - app2

  app1:
    build: .
    command: bash -c "rm -f /app/tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    environment:
      RAILS_ENV: development
      DATABASE_URL: postgres://postgres:postgres@db:5432/app_development
    volumes:
      - .:/app
    depends_on:
      - db

  app2:
    build: .
    command: bash -c "rm -f /app/tmp/pids/server.pid && bundle exec rails s -p 3001 -b '0.0.0.0'"
    environment:
      RAILS_ENV: development
      DATABASE_URL: postgres://postgres:postgres@db:5432/app_development
    volumes:
      - .:/app
    depends_on:
      - db

volumes:
  db-data:
```
{: file='docker-compose.yml'}

```conf
upstream app_servers {
  server app1:3000;
  server app2:3001;
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


```shell
#!/bin/bash
docker-compose up --remove-orphans -d
```
{: file='start.sh'}

---

```yml
default: &default
  adapter: postgresql
  pool: 5

development:
  <<: *default
  database: finances_db
  host: <%= ENV['POSTGRES_DB'] %>
  username: <%= ENV['POSTGRES_USER'] %>
  password: <%= ENV['POSTGRES_PASSWORD'] %>
```
{: file='database.yml'}
