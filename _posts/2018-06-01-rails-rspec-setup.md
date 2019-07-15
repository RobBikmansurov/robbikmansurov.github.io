---
layout: post
title:  Настройка среды тестирования Rails/RSpec
date:   2018-06-01 13:28:44 +0500
tags:   rails rspec
categories: rails
keywords: rspec, ruby on rails, tests
---

Для развёртывания стандартной среды тестирования обычно нужно установить и настроить привычные инструменты.

Добавляем указанные ниже гемы в `Gemfile`, запускаем `bundle install`, чтобы установить эти библиотеки локально. 

## RSpec

[RSpec](https://github.com/rspec/rspec-rails) - основной инструмент для unit-тестирования. 
А [вот здесь](https://relishapp.com/rspec/rspec-rails/docs) хороший справочник по использованию RSpec в Rails.

```ruby
group :development, :test do
  ...
  gem 'rspec-rails', '~> 3.5'
end
````

Запускаем `bundle install`, а затем `bundle exec rails generate rspec:install`, чтобы создать структуру каталогов `spec/`. 

## Shoulda matchers

[Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers) облегчит и ускорит тестирование за счет своих врапперов для валидаций, ассоциаций и редиректов. 

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

[factory_bot](https://github.com/thoughtbot/factory_bot_rails) - отличная вещь для наполнения базы данных разнообразными объектами. Можно использовать как для тестов, так и для начальной инициализации базы данных и заселения её объектами для разработчиков (`db/seeds.rb`).

```ruby
group :development, :test do
  ...
  gem 'factory_bot_rails'
end
```

После установки нужно создать свои фейковые модели в `spec/factories/`

## Database Cleaner

[Database Cleaner](https://github.com/DatabaseCleaner/database_cleaner) незаменим для поддержания нашей тестовой базы данных в здоровом состоянии. Имеет развитые средства управления стратегиями очистки.

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

## rails-controller-testing

[rails-controller-testing](https://github.com/rails/rails-controller-testing) расширяет наши тесты контроллеров операторами `assigns` и `assert_templates`.

## Cucumber

[Cucumber Rails](https://github.com/cucumber/cucumber-rails) нужен для приемочных (asseptans) тестов

```ruby
group :development, :test do
  ...
  gem 'cucumber-rails', require: false
end
```

`bundle exec rails generate cucumber:install` создаст структуру `spec/features/`

На этом процесс подготовки можно считать законченным и можем переходить к написанию тестов.
