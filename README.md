# Cells::Mailer

Provides mail functionality for [Cells](https://github.com/apotonick/cells/) via the [Mail](https://github.com/mikel/mail) gem.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'cells-mailer'
```

And then execute:

    $ bundle

## Configuration

### Global configuration

```ruby
Cell::Mailer.configure do
  to "nick@trailblazer.to"
  from "timo@schilling.io"
  subject "Nick ruls!"
  mail_options deliver_method: :smtp
end
```
`mail_options` will be passed to the `Mail` gem, for details take a look on the [Mail](https://github.com/mikel/mail) gem.

### Context configuration

See next chapter.

## Usage

```ruby
class UserNotificationCell < Cell::ViewModel
  include Cell::Mailer
  property :user_name

  def show
    "Hello #{user_name}"
  end
end

UserNotificationCell.(user).deliver(from: "foo@example.com", to: user.email, subject: "Hello")
```

### Body

Equal to Cells, you can deliver (render) different states of your Cell:

```ruby
class UserNotificationCell < Cell::ViewModel
  include Cell::Mailer
  property :user_name

  def welcome
    "Hello #{user_name}"
  end
end

UserNotificationCell.(user).deliver(..., method: :welcome)
```

I don't know why you should use it, but you can also pass in a body as argument.

```ruby
UserNotificationCell.(user).deliver(..., body: "Hello user")
```

### Instance method arguments

You can use instance methods to receive the value for `to`, `from` and `subject`.

```ruby
class UserNotificationCell < Cell::ViewModel
  include Cell::Mailer
  property :user_name

  def subject
    "Hello #{user_name}"
  end
end

UserNotificationCell.(user).deliver(..., subject: :subject)
```

### Class level configurations

You can use class level configurations for `to`, `from`, `subject` and `mail_options`.
`mail_options` are passed to `Mail`, could be used to configure `Mail#delivery_method` per Cell class.

```ruby
class UserNotificationCell < Cell::ViewModel
  include Cell::Mailer

  mailer do
    from "nick@trailblazer.to"
    subject "nick loves good code!"
    mail_options delivery_method: :smtp
    format :html # or :text
  end
end

UserNotificationCell.(user).deliver(to: "timo@schilling.io")
```

This configurations will be inherited.

## Roadmap

* multipart emails
* attachments
