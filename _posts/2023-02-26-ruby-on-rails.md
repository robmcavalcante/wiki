---
title: RubyOnRails
author: robson
date: 2023-02-21 22:09:00 +0800
categories: [rubyonrails]
tags: [ruby, rubyonrails, programming, commands]
---

# RubyOnRails

## Gems
### Development
- faker
- rubocop-rails
- rubycritic
- rails_db
- byebug

### Utils
- ransack
- pagy

### Others
- brakeman

## SETUP
`config/application.rb`
```
config.generators do |g|
  g.system_tests = nil
  g.view_specs false
  g.helper_specs false
  g.test_framework false

  g.stylesheets false
  g.javascripts false
  g.helper false
  g.skip_routes true
end
```

## TEST A SNIP OF CODE VIA THE CONSOLE
`app/services/myclass.rb`
```ruby
class MyClass
  class << self
    def hello 
      puts 'Hello World!'
    end
  end
end
```
```bash
$ Teste.bar
```

## PostgreSQL Database Container
```bash
$ docker run --name db1_postgres -e POSTGRES_DB=project_db -d postgres
```

`config/database.yml`
```bash
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  host: localhost

development:
  <<: *default
  database: project_db
  user: postgres
  password: postgres
```