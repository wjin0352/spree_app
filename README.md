Spree is an ecommerce framework for Ruby on Rails that is currently at version 2.2.2. I often find that individuals come to spree when Magento or Shopify just can’t serve their needs anymore and they need an even more custom solution.

To get started, we first need to install spree:

$ gem install spree
OK, that was fun, now let’s go ahead and make a new rails project:

$ rails _4.0.5_ new spree_app
Now that you have that done, try this out:

$ spree install spree_app
Or, if you’ve already navigated into spree_app:

$ spree install .
You will get a number of questions:

Would you like to install the default gateways? (Recommended) (yes/no) [yes]
Would you like to install the default authentication system? (yes/no) [yes]
Would you like to run the migrations? (yes/no) [yes]
Would you like to load the seed data? (yes/no) [yes]
Would you like to load the sample data? (yes/no) [yes]
Right now, please answer yes (the default) to all of them. When starting a new spree project, I often do spree install . -A which auto-accepts all parameters.

Now, as its installing, it will stop and ask you to create an Admin user:

Create the admin user (press enter for defaults).
Email [spree@example.com]:
Password [spree123]:
Just push the default options so that we can get going.

Throughout the installation, you might have noticed this warning:

[WARNING] You are not setting Devise.secret_key within your application!
You must set this in config/initializers/devise.rb. Here's an example:

Devise.secret_key = "a0c5e56c8764a93b03fce02083f4aed18aef63ef4d7be33f351accb002d4e2187ac6ba4a78090c341281f51a6abebd4fe499"
Now I’m going to tell you how to solve this. If you open up your Gemfile it should look like this:

source 'https://rubygems.org'

gem 'rails', '4.0.5'
gem 'sqlite3'

gem 'sass-rails', '~> 4.0.2'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.0.0'

gem 'jquery-rails'
gem 'turbolinks'
gem 'jbuilder', '~> 1.2'

group :doc do
  gem 'sdoc', require: false
end

gem 'spree', '2.2.2'
gem 'spree_gateway', github: 'spree/spree_gateway', branch: '2-2-stable'
gem 'spree_auth_devise', github: 'spree/spree_auth_devise', branch: '2-2-stable'
Notice at the bottom there were 3 gems that got installed. spree is the main project and is definately what you wanted to install. There’s also two more: spree_gateway and spree_auth_devise. spree_gateway basically handles active merchant, stripe, checks, stuff that allows you to receive money. spree_auth_devise is used for authenticating users. Its different from devise in that it requires you to be running devise; since bundler detects that you don’t have it in your Gemfile, it does it for you - the magic of Bundler.

Because spree_auth_devise is its own gem, it actually never got installed fully when you ran spree install. Therefore, we have to do another installation for spree_auth_devise. Here’s how you install spree_auth_devise:

$ bundle exec rails g spree:auth:install
you will see the error pop up one more time and then directly after that you get:

create  config/initializers/devise.rb
And there we go, its gone. Alternatively, you could have also created the initializer yourself and copy and pasted the devise secret key. To generate secret keys, you can use rake secret like so:

$ rake secret
9ba50700679fc17712ce1a1538bb9dd05c31038e12d1155cb8d437a9dd69a0a2fc8b7f9425f254c77d5bfa0587c9e50df0931f3803d947eb5fe13c7d1b6cec00
And there you go, now lets start the application:

$ rails s
IF you head over to localhost:3000 you will see the spree skeleton.