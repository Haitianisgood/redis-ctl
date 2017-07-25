Manage your Redis Clusters in a web GUI

![Redis Overview](http://zlo.gs/image_data/8fb3021522baf6414c107cfd3fc0ad406461d5e5)

Commit a cluster migration from a HTML form

![Slot Migrating](http://zlo.gs/image_data/5dc602f756975f97a9264e6e7a94a3ad08518a2f)

Display cluster nodes in a tidy table

![CLUSTER NODES](http://zlo.gs/image_data/4fda367ddacd1501337bc725002dc98384083179)

RedisCtl is a set of Python toolkit with a web UI based on Flask that makes it easy to manage Redis and clusters.

# Overview

RedisCtl contains

* a web UI that displays Redis status and receives commands
* a daemon that polls each Redis and collect info and runs tasks like slots migrating for clusters

And support thirdparty utilities including

* Alarm: issue messages when Redis / proxy unreachable or cluster is down
* Statistic: record information link memory / CPU usage, clients count, commands completed, etc; by default [OpenFalcon](https://github.com/open-falcon) is included in the source (require an extra `pip install requests` to work with OpenFalcon module)
* Containerization: deploy Redis / proxy in container

You could [make your own overridings](https://github.com/HunanTV/redis-ctl/wiki/Customize_App) by implementing several interfaces. ([in Chinese](https://github.com/HunanTV/redis-ctl/wiki/WIP_v0_9_customize_app_zh)).

# Setup

## environment

   1.python2.7
   2.python27-pip
   3.mysql5.5(Personally, I confirmed)
   
First, install Python-dev header files and libs

    # debain / ubuntu
    apt-get install python-dev

    # centos
    yum install python-devel

    if it dont's work,then:
    
    yum -y install python27-devel

Then clone this project and cd in to install dependencies

    pip install -r requirements.txt

Run with all configurations default

    python main.py

To configure the programs, both configuration source file and environment variables (convenient for docker deployment) are applicable

To use a configure file, copy `override_config.py.example` to `override_config.py`, change anything you want. This file would be imported and override any default config or env vars in `config.py` if available.

To use env vars, like

    MYSQL_USERNAME=redisctl MYSQL_PASSWORD=p@55w0rd python main.py

Check `config.py` for configurable items.

Run the daemon that collects Redis info

    python daemon.py

Also you could use similar ways to configure daemon, just like setup up the main server.

# IPC

The server and daemon uses `/tmp/details.json` and `/tmp/poll.json` as default IPC files. You could change the directory for those temp files by passing the same `PERMDIR` environ to the web application and the daemon.

# NOT SUPPORT

This project is not support redis's "Password Authentication"  because of using redis-trib(itself not support password authentication)

# Q&A

1. About mysql-devel

The redis-ctl can support mysql5.5(Personally, I confirmed) but not support 5.1.If you use mysql5.5 on centos：

    yum list |grep mysql |grep devel
    yum -y install mysql55-devel
 
