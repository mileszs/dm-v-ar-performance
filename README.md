# DataMapper v. ActiveRecord Performance

## Setup

In both apps there is a very basic User model. They are using the same database (on MySQL). The db/seeds.rb file in the DataMapper app creates 10000 user records.

I used the recommended app template to generate the DataMapper app:

    rails new dm-perf-app -m http://datamapper.org/templates/rails.rb
    bundle install
    rails g scaffold User email:string
    rake db:setup
    rake db:seed

The ActiveRecord app is similar, but uses the same database, so it was less work:

    rails new ar-perf-app
    bundle install
    rails g scaffold User email:string

To test performance, I used variations of:

    # DataMapper
    env RAILS_ENV=production rails benchmarker "10000.times { |i| u = User.all(:id => i).first; u.email if u }"

    # ActiveRecord
    env RAILS_ENV=production rails benchmarker "10000.times { |i| u = User.where(:id => i).first; u.email if u }"

... which I stole from [Luke Ludwig's "Rails 3 Performance - Not Good Enough](http://blog.tstmedia.com/news_article/show/86942?referrer_id=308069) article.
