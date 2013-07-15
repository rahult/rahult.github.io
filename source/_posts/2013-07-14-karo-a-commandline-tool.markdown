---
layout: post
title: "Karo - A command line companion for a Rails application"
date: 2013-07-14 22:10
comments: true
sharing: true
categories: [CLI, Ruby, Ruby on Rails]
---

**Karo** is a command line companion for a rails application which makes performing routine commands locally or on the server easier.

Github - [https://github.com/rahult/karo](https://github.com/rahult/karo)
Website - [http://rahult.github.io/karo/](http://rahult.github.io/karo/)

Example of things it can do, for rest please refer to ```karo help```

```bash terminal
karo db pull # Will sync the production MySQL database on to local machine
karo db console # Will open MySQL console on the server
karo console # Will open Rails console on the server
karo assets pull -e staging # Will sync the dragonfly assets from staging on to the local machine
karo log -f # Will tail the production log with -f argument
```

You can also write your custom commands

```bash terminal
karo server {command} # Will try to find a command from .karo.yml or execute the one provided on the server
karo client {command} # Will try to find a command from .karo.yml or execute the one provided on the client
```

## Few Assumptions

- You have SSH access to your servers
- You have the [Capistrano](https://github.com/capistrano/capistrano) deploy directory structure on your server  
  [deploy_to]  
  [deploy_to]/releases  
  [deploy_to]/releases/20080819001122  
  [deploy_to]/releases/...  
  [deploy_to]/shared  
  [deploy_to]/shared/config/database.yml  
  [deploy_to]/shared/log  
  [deploy_to]/shared/pids  
  [deploy_to]/shared/system  
  [deploy_to]/current -> [deploy_to]/releases/20100819001122  
- You are using MySQL as your database server
- You are using Dragonfly to store your assets

I am working on supporting other databases and assets managers for future releases

## Installation

Karo is released as a Ruby Gem. The gem is to be installed within a Ruby
on Rails application. To install, simply add the following to your Gemfile:

```ruby Gemfile
gem 'karo'
```

After updating your bundle, you can use Karo function from the command line

## Usage (command line)

```bash terminal
karo help
```

## Default configuration file (.karo.yml)

```yml .karo.yml
production:
  host: example.com
  user: deploy
  path: /data/app_name
  commands:
    server:
      memory: watch vmstat -sSM
      top_5_memory: ps aux | sort -nk +4 | tail
    client:
      deploy: cap production deploy
staging:
  host: example.com
  user: deploy
  path: /data/app_name
  commands:
    server:
      memory: vmstat -sSM
    client:
      deploy: ey deploy -e staging -r staging
```
