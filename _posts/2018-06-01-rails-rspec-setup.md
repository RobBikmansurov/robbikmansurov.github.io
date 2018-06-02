---
layout: post
title:  Настройка среды тестирования Rails/Rspec
date:   2018-06-01 13:28:44 +0500
tags:   rails rspec
categories: rails
---

Для развёртывания стандартной среды тестирования обычно нужно установить и настроить привычные интсрументы. 

## Rspec

Добавим в Gemfile:

```ruby
group :development, :test do
  ...
  gem 'rspec-rails', '~> 3.5'
end
````

Запускаем `bundle install` для установки гема

Затем `bundle exec rails generate rspec:install`

## Shoulda-matchers

```ruby
group :development, :test do
  ...
  gem 'shoulda-matchers', '~> 3.1'
end
```

## FactoryBot

```ruby
group :development, :test do
  ...
  gem 'factory_bot_rails'
end
```

## Database Cleaner

```ruby
group :test do
  gem 'database_cleaner'
end
```

```ruby
# spec/rails_helper.rb
RSpec.configure do |config|

  config.before(:suite) do
    DatabaseCleaner.strategy = :transaction
    DatabaseCleaner.clean_with(:truncation)
  end

  config.around(:each) do |example|
    DatabaseCleaner.cleaning do
      example.run
    end
  end
end
```

## Cucumber


[bp1step]: http://bp1step.ru
[shoulda-matchers]: https://github.com/thoughtbot/shoulda-matchers

