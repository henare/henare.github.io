---
layout: post
status: publish
published: true
title: Load RSpec fixtures in Rails
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 543
wordpress_url: http://www.henaredegan.com/blog/?p=543
date: '2014-04-25 13:56:57 +1000'
date_gmt: '2014-04-25 03:56:57 +1000'
categories:
- Hacking
tags:
- ruby
- rails
- RSpec
- Fixtures
- test
- data
comments: []
---
If you're still using fixtures in your RSpec Rails tests for some reason you might want to load them into an environment to look at. For example, to ensure the data is displaying in the application like you intend.

Rails expects you're using Test::Unit so it expects the fixtures to live in `test/fixtures`. Here's how you load fixtures from RSpec into your 'test' environment:

`bundle exec rake db:fixtures:load RAILS_ENV=test FIXTURES_PATH=spec/fixtures`

~~You get a deprecation warning when running this. It says you should set this with `ActiveRecord::Tasks::DatabaseTasks.fixtures_path = '/path/to/fixtures'` instead but I haven't worked out where you'd put that for this kind of use (suggestions in the comments greatly appreciated!).~~ _Update: the deprecation warning was [removed in Rails 4.1](https://github.com/rails/rails/commit/3d865c1bcdb5a9c1ecf7a968312752d8f6178f8b)._
