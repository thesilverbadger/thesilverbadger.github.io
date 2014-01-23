---
layout: post
title:  "Running Ruby on Rail on a Raspberry Pi"
date:   2013-08-15 10:18:00
categories: ruby on rails, raspberry pi, rpi, rails, apache, passenger
---

<i>Disclaimer: I'm not a Linux sysadmin, nor have I been writing Ruby on Rails for long. This post is just what has worked for me, and there may be glaring issues with what I have done that I am unaware of. Use at your own risk!</i>

This has been a real learning experience for me, as I've tackled two subjects that I am by no means an expert on: Linux and Ruby on Rails. For those unfamiliar with [Ruby on Rails] (RoR), it is a popular web development framework based on the Ruby language. It is very close conceptually to the ASP.NET MVC framework that I am very familiar with, and as it runs best on Linux it seems a good match for my skills and my Raspberry Pi.

I'll assume that you can connect to your RPi, and are reasonable familiar with SSH and navigating around the file system. I am also assuming you're starting from a bare install of Raspbian with no existing web server or Ruby version installed.

[Ruby on Rails]: http://rubyonrails.org/

### 1) Install RVM
First thing to do is to install a Ruby manager like [RVM]. This allows you to easy install Ruby alongside different versions (should you need to), and greatly simplifies gem management later on.
I followed the instructions for installing Ruby system-wide:

> sudo curl -L https://get.rvm.io | sudo bash -s stable

Follow the onscreen instructions - you may need to reload your ssh environment once done.
When finished, you should be able to run

> rvm -v

to see the version of RVM installed.

[RVM]: https://rvm.io/

### 2) Install Ruby
It's now simple to install the version of Ruby you require (I've gone for the latest stable release), as RVM will handle finding the right version for your architecture.

> rvm install ruby

There isn't a package for ARM, so on the RPi RVM downloads and compiles Ruby from source. This may take some time, so be patient!

### 3) Add ~/.gemrc
You should now have Ruby installed system-wide (you can check this by logging in as another user and running 

> ruby -v

Ruby has a great package management system called gems (think Nuget in the .NET world), which makes managing packages nice and simple. Most packages come with documentation, but as we're installing this on a 'production' enviroment we can avoid installing this (and the time it takes on a RPi) by editing the <i>~/.gemrc</i> file.

> sudo nano ~/.gemrc

Add the following line and save:

> gem: --no-ri --no-rdoc

### 4) Install Rails and Passenger
Rails is a gem, and like all gems can be installed using the gem management system:

> gem install rails

Same with Passenger

> gem install passenger

### 6) Apache and Passenger
Far and away the simplest method for running Rails apps is [Phusion Passenger]. Now that the passenger gem has been installed, running

> passenger-install-apache2-module

will guide you through installing and configuring Apache2, along with any required distro packages for your OS.  Note the warning about adding a swap file - the Raspberry Pi only has 512MB RAM, and Passenger recommends creating a swap file whilst compiling.  I've never tried without one and always follow the advice, so if you have the spare SD space it's probably wise to follow the swap instructions.
Take a note of the configurations values once completed, as you'll need to add them to <i>apache2.conf</i> in <i>/etc/apache2</i>. Also make a note of the example virtual host file.

[Phusion Passenger]: https://www.phusionpassenger.com/

### 7) Install Javascript runtime
You will require a Javascript runtime for asset compilation - everyone's favourite today seems to be nodejs, and there is a package available (although slightly old) for the Raspberry Pi

> sudo apt-get install nodejs

### 8) Create apps directory in home and create a rails app
I normally create two directories in the home directory for my user - <i>apps</i> and <i>www</i>.

> mkdir apps

You can now either create a new rails app or deploy an existing one using git, ftp, scp or capistrano. Put the files in <i>/home/_your-user_/apps</i>, and then copy the default virtual hosts file in <i>/etc/apache2/sites-available</i>

> sudo cp default default.bak

Now edit the file and copy the example virtual host from step 6. Change the settings as necessary to point to the application you've deployed in /home/your-user/apps. Don't forget to change the <i>ServerName</i> if you want multiple virtual hosts, and both <i>DocumentRoot</i> and <i>Directory</i> to point to the public directory inside your rails application.
That should be it - visit your RPi's IP address or host in a browser, and you should see your application.

If you run into problems, you can check for errors in <i>your_application_directory/log</i>