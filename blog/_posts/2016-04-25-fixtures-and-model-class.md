---
layout:      post
title:       Seeding databases using more complex Rails fixtures
description: A walkthrough when fixtures do not match their models name.
date:        2016-04-25 23:00
categories:  rails ruby
---

Ruby on Rails applications can be easily tested using fixtures to load the initial data. Each fixture filename would normally match the model name, and any exception to that rule would use the [`set_fixture_class`](http://apidock.com/rails/ActiveRecord/TestFixtures/ClassMethods/set_fixture_class) method to map the
fixture and the model.

The `set_fixture_class` solution works fine for testings, but it fails when seeding databases with `rails db:fixtures:load` (a.k.a `rake db:fixtures:load`). The mapping created by the method is only available to tests because it is defined directly into the tests Ruby code. Therefore the `db:fixtures:load` task cannot use it to load the fixtures.

A solution has recently been [introduced to the Rails 5](https://github.com/rails/rails/pull/20574) and will allow the mapping to be done from the fixture file itself. The following case loads the `User` model from the `accounts` table:

~~~ruby
# schema.rb
create_table "accounts", force: :cascade do |t|
  t.string   "name"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
end

# user.rb
class User < ActiveRecord::Base
  self.table_name = 'accounts'
end
~~~

Using the `model_class` property from `_fixture` in the YAML file, the `accounts` fixtures can now instantiate `User` records:

~~~yml
# accounts.yml

_fixture:
  model_class: User

admin:
  name: 'Foo'
~~~

That allows `db:fixtures:load` to recognize the model and to seed the records. The setting is also transparent to the model associations. So if there is a scenario where a `user` can have many `roles`, it can be described in the `roles` fixture without any mention to `accounts`:

~~~ruby
# schema.rb
create_table "roles", force: :cascade do |t|
  t.string   "name"
  t.integer  "user_id"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
end

# role.rb
class Role < ApplicationRecord
  belongs_to :user, inverse_of: :roles
end

# user.rb
class User < ActiveRecord::Base
  self.table_name = 'accounts'
  has_many :roles, inverse_of: :user, dependent: :destroy
end
~~~

And for the `roles` fixtures:

~~~yml
# roles.yml
admin_role:
  name: 'admin'
  user: admin
~~~

Back to the tests, the `set_fixture_class` method will overwrite any `model_class` setting in fixtures for a more refined control.
