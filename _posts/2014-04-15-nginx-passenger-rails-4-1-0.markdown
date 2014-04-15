---
layout: post
title:  "Rails 4.1.0 - Nginx Bad Gateway with Passenger"
date:   2014-04-15 20:11:00
categories: Rails 4.1.0, Nginx, Bad Gateway, Passenger
---

I'm writing a little issue tracking application, and as it's a new app I'm using the new shiny Rails 4.1.0.  However, whilst the application runs fine locally using Webrick I've been unable to get it working when deployed onto a live (staging) environment running Passenger and Nginx (using the Ubuntu package available via the <a href='http://www.modrails.com/documentation/Users%20guide%20Nginx.html#install_on_debian_ubuntu'>Phusion Passenger</a> site).

When deployed and configured, Nginx reports a 502 Bad Gateway exception.  Digging throught the Nginx error log (in `/var/log/nginx`), the error becomes clear:

Exception RuntimeError in Rack application object (Missing `secret_key_base` for 'production' environment, set this value in `config/secrets.yml`)

Opening secrets.yml shows that there is no value for production:

{% highlight ruby %}
development:
  secret_key_base: **not_for_prying_eyes**

test:
  secret_key_base: **not_for_prying_eyes**

# Do not keep production secrets in the repository,
# instead read values from the environment.
production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
{% endhighlight %} 

You can either set an environment variable, or directly configure a value here (you can run `rake secret` to generate a key).  Just make sure that if you do the latter, you've added secrets/yml to your .gitignore!
