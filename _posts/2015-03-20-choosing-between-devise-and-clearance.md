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
