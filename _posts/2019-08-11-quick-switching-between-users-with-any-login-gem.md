---
layout: post
title: "Быстрое переключение между пользователями с помощью AnyLogin gem"
date: 2019-08-11T16:05:00Z
categories: Rails

---
Я думаю, всем знакомо, когда в системе есть несколько пользователей (например, с разными ролями) и нужно сделать, что-то одним, а потом другим пользователем.
И тут, начинается постоянный логин/логаут туда сюда, либо, еще несколько вкладок инкогнито режима с несколькими пользователями.
Вобщем, такая себе быстрая разработка.

На помощь приходит [https://github.com/igorkasyanchuk/any_login](https://github.com/igorkasyanchuk/any_login]).

После установки в нижнем левом углу пояляеться иконка, с помощью которой можно легко и быстро переключать пользователей:

![AnyLogin gem use case](/assets/any_login.png "AnyLogin gem use case")

## Установка

Я думаю [здесь](https://github.com/igorkasyanchuk/any_login]) все и так ясно, в кратце:

1. Добавляем в gemfile 

    ```
    gem 'any_login'
    ```
2. В главный лайаут (например, ```app/views/layouts/application.html.erb``` если у вас rails и erb) добавляем: 

    ```erb
    ...
    </html>
    <%= = any_login_here if defined?(AnyLogin) %>
    ```
3. Вобщем все. Затем в левом нижнем углу появиться иконка, позваляющая вибирать пользователей как на картинке выше.

Если у вас Rails + Devise больше ничего делать не нужно. 
По умолчанию этот gem включен только в development mode.

### Полезное

* Для того, чтоб вот вырубить для прода, вот так, прям на 100% делаем так:

```ruby
# Initializer: config/initializers/any_login.rb
AnyLogin.setup do |config|
  config.enabled = false
end
```
* Кастомизация

 ```ruby
rails g any_login initializer
```

Можно настроить более детально, например: лимит количества юзеров в дропдауне, адрес для редиректа после удачного входа и тд.

Все настройки тоже есть [здесь](https://github.com/igorkasyanchuk/any_login]) в разделе Options.

Еще пользователей в дропдауне можно сортировать сразу по группам, как-то так:

```ruby
# Initializer: config/initializers/any_login.rb
AnyLogin.setup do |config|
  config.collection_method = :grouped_collection_by_role
end

# User class: app/models/user.rb
class User < ActiveRecord::Base
  def self.grouped_collection_by_role
    {
      'admin'     => User.admins.limits(10),
      'moderator' => User.moderators.limit(10),
      'user'      => User.users.limit(10)
    }
  end
end
```

### Ошибки
Если вдруг у вас ошибка при запуске сервера типа такой:

```
'method_missing': undefined method 'assets'
```

Значит, скорей всего, необходимо добавить для правильной работы в ```config/application.rb```

```ruby
require "sprockets/railtie"
```
