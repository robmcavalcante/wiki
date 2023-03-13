---
title: RubyOnRails
author: robson
date: 2023-02-26 22:09:00 +0800
categories: [rubyonrails]
tags: [ruby, rubyonrails, development, commands]
---

# RubyOnRails

## Gems
### Development
- [solargraph-rails](https://github.com/iftheshoefritz/solargraph-rails)
- [rubocop-rails](https://github.com/rubocop/rubocop-rails)
- [rubocop-performance](https://github.com/rubocop/rubocop-performance)
- [rubocop-rspec](https://github.com/rubocop/rubocop-rspec)
- [rubocop-capybara](https://github.com/rubocop/rubocop-capybara)
- [rspec-style-guide](https://github.com/rubocop/rspec-style-guide)
- [rails-style-guide](https://github.com/rubocop/rails-style-guide)

---

- [faker](https://github.com/faker-ruby/faker)
- [rspec-rails](https://github.com/rspec/rspec-rails)
- [factory_bot_rails](https://github.com/thoughtbot/factory_bot_rails)
- [capybara](https://github.com/teamcapybara/capybara)

---

- [rubycritic](https://github.com/whitesmith/rubycritic)
- [rails_db](https://github.com/igorkasyanchuk/rails_db)
- [byebug](https://github.com/deivid-rodriguez/byebug)

### Utils
- [ransack](https://github.com/activerecord-hackery/ransack)
- [pagy](https://github.com/ddnexus/pagy)

### Others
- [brakeman](https://github.com/presidentbeef/brakeman)

## SETUP
`config/application.rb`
```ruby
config.generators do |g|
  g.helper false
  g.stylesheets false
  g.javascripts false
    
  g.skip_routes true
  
  g.test_framework false
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
