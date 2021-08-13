---
layout: post
title:  "RabbitMQ notes"
date:   2021-08-13 18:09:22 -0500
categories: RabbitMQ
---

# Install RabbitMQ on Mac By Using Homebrew #

``` shell
brew install rabbitmq
```

# Operations

RabbitMQ server scripts and CLI tools location:` /usr/local/Cellar/rabbitmq/<version>/`

## Install RabbitMQ Plugin ##

``` shell
cd /usr/local/Cellar/rabbitmq/<version>/
sudo sbin/rabbitmq-plugins enable rabbitmq_management
```

## Set the Environmental variable ##

```shell
vim ~/.bash_profile
```

add these two lines below into it.

```shell
export RABBIT_HOME=/usr/local/Cellar/rabbitmq/3.8.9_1
export PATH=$PATH:$RABBIT_HOME/sbin
```

use `:q` to quit `vim` and make `.bash_profile` effective

```shell
source ~/.bash_profile
```

## Launch RabbitMQ ##

```shell
rabbitmq-server -detached #run RabbitMQ in background
rabbitmqctl status #check status
#as we have installed the RabbitMQ
#we can use http://localhost:15672 with username `guest` and password `guest` to view the status.
rabbitmqctl stop #close rabbitmq
```

 <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/rabbitmq1.png" alt="image-20210812203832321" style="zoom:20%;" />

## Errors ##

If we meet the error that `20:33:22.346 [error] Error when reading /Users/<user>/.erlang.cookie: eacces`, this may happen because of the privilege of `.erlang.cookie`, you can use `sudo rabbit-server` to launch it, or change the owner of `.erlang.cookie`.

