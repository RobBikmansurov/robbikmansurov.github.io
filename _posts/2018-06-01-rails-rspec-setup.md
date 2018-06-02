---
layout: post
title:  Настройка среды тестирования Rails/Rspec
date:   2018-06-01 13:28:44 +0500
tags:   rails rspec
categories: rails
---

Для развёртывания стандартной среды тестирования обычно нужно установить и настроить привычные инструменты. Добавляем указанные ниже гемы в `Gemfile`, запускаем `bundle install`, а затем

## Rspec

Это основной инструмент unit-тестирования. Добавим в Gemfile:

```ruby
group :development, :test do
  ...
  gem 'rspec-rails', '~> 3.5'
end
````

Запускаем `bundle install`, а затем `bundle exec rails generate rspec:install`, чтобы установить гем локально и создать структуру каталогов `spec/`. 

## shoulda-mmatchers

Облегчит жизнь тестировщику за счет своих врапперов для валидаций, ассоциаций и редиректов. 

```ruby
group :development, :test do
  ...
  gem 'shoulda-matchers', '~> 3.1'
end
```

В `spec/rails_helper.rb` нужно добавить его настройки:
```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec

    with.library :active_record
    with.library :active_model
    with.library :action_controller
    with.library :rails
  end
end
```

## FactoryBot

Незаменим для наполнения базы данных разнообразными объектами.

```ruby
group :development, :test do
  ...
  gem 'factory_bot_rails'
end
```

После установки нужно создать свои фейковые модели в `spec/factories/`

## Database Cleaner

Незаменим для поддержания нашей тестовой базы данных в здоровом состоянии. Имеет развитые средства управления стратегиями очистки.

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
[factory_bot]: https://github.com/thoughtbot/factory_bot_rails
