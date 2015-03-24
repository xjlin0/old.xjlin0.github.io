---
layout: post
title: "choosing between devise and clearance"
description: "user authentication comes handy"
category: tech
tags: [gem, rails]
---
{% include JB/setup %}
### These gems can really help your rails app on user control <img src="/assets/imgs/users.jpg"  alt="major incrediable roles" width="20%"/>

<hr>

Gem names   |  ***[plataformatec's_Device](https://github.com/plataformatec/devise)***   |  ***[thoughtbot's Clearance](https://github.com/thoughtbot/clearance)***  |
:--------- |:----------------------------: | :--------------------------: |Summary    | MVC/10modules&more extentions |    small & simple
Install    | rails generate devise:install | rails generate clearance:install
           | rails generate devise MODEL   |   (create migration for you)
|other Auth|    by OmniAuth                | nil
|mailComfirm|    bkg ActiveJob/ActionMailer |  bkg ([deliver_later](http://edgeapi.rubyonrails.org/classes/ActionMailer/MessageDelivery.html#method-i-deliver_later))
|PassWdReset| Recoverable&Confirmable       | Yes
|Session    |cookie Timeoutable&Rememberable| Yes (by Rack session)
|Sign-up    |Validatable & Lockable         |    SignInGuard
|I18n       |   yes                         | yes(even path names)
|Track/log  |   Yes                         | nil
| testing   |  Test helpers                 | yes with factory_girl_rails
|           |                               |(rails generate clearance:specs)

<hr>

### [Hre is a great tutorial of Device can be found on Ruby girls](http://guides.railsgirls.com/devise/)

### Here's how I setup the clearance
{% highlight ruby %}
rails new clearance -B
echo "gem 'clearance'" >> clearance/Gemfile
echo "gem 'bcrypt'"      >> clearance/Gemfile
echo "gem 'faker'"       >> clearance/Gemfile
cd clearance && bundle install
rails generate scaffold User username email password
echo "10.times { User.create(username: Faker::Internet.user_name, email: Faker::Internet.email, password: '1') }" >> db/seeds.rb
rake db:migrate
rails generate clearance:install
rake db:migrate
rake db:seed
open http://localhost:3000/users/
rails s
{% endhighlight %}
<pre>
Clearance post installation
*******************************************************************************

Next steps:

1. Configure the mailer to create full URLs in emails:

    # config/environments/{development,test}.rb
    config.action_mailer.default_url_options = { host: 'localhost:3000' }

    In production it should be your app's domain name.

2. Display user session and flashes. For example, in your application layout:

    <% if signed_in? %>
      Signed in as: <%= current_user.email %>
      <%= button_to 'Sign out', sign_out_path, method: :delete %>
    <% else %>
      <%= link_to 'Sign in', sign_in_path %>
    <% end %>

    <div id="flash">
      <% flash.each do |key, value| %>
        <div class="flash <%= key %>"><%= value %></div>
      <% end %>
    </div>

3. Migrate:

    rake db:migrate

*******************************************************************************
</pre>