---
title: Linux自定义服务
date: 2024-03-22 14:38:45
tags: [systemctl,service]
categories: 
    - Linux
---

## Linux自定义服务

自定义服务可以实现应用后台运行

对于会阻塞命令行的任务，将其创建为服务可以保持后台持续运行，并且实现开机自动启动。

## 服务文件

服务文件是以`.service`结尾的文件，存放在指定的目录中可以被系统识别为服务。

### 存储路径

**一般自定义的服务可以存放在`/etc/systemd/system/`目录下。**

还有一些其他的目录用来存放服务文件

1. `/etc/init.d`
   这个目录通常用于存放 SysVinit 系统的服务启动脚本。每个脚本都是一个独立的可执行文件，负责启动、停止和管理特定的服务。

2. `/etc/init/`
   这个目录用于存放 Upstart 系统的服务配置文件。与 systemd 不同，Upstart 使用简单的文本文件来描述服务及其运行参数。

3. `/usr/lib/systemd/system/`
   在一些发行版中，系统提供的服务文件可能存放在这个目录中。这些服务文件通常由发行版提供，并且不应该由用户修改。

### 服务文件模板

```ini
[Unit]
Description=My Service # 服务描述
After=network.target # 服务依赖，表示本服务应在哪些服务之后启动

[Service]
Type=simple # 服务类型，常见的有 simple 和 forking
ExecStart=/path/to/my-service.sh         # 启动服务命令
ExecStop=/path/to/my-service-stop.sh     # 停止服务命令
ExecReload=/path/to/my-service-reload.sh # 重启服务命令
User=myuser   # 运行服务的用户
Group=mygroup # 运行服务的用户组
WorkingDirectory=/path/to/my-service # 工作目录
PIDFile=/var/run/my-service.pid      # PID文件目录
StandardOutput=/var/log/my-service/stdout.log # 标准输出日志文件路径
StandardError=/var/log/my-service/stderr.log  # 标准错误日志文件路径
Restart=always # 指定服务在失败或意外退出时是否应该重新启动。
RestartSec=10  # 重启延迟时间，单位秒
StartLimitIntervalSec=60 # 启动频率限制

[Install]
WantedBy=multi-user.target # 服务应被哪些运行级别启动
```

### 服务操作命令

* 启用服务：`sudo systemctl enable <service_name>`
* 禁用服务：`sudo systemctl disable <service_name>`
* 启动服务：`sudo systemctl start <service_name>`
* 停止服务：`sudo systemctl stop <service_name>`
* 重启服务：`sudo systemctl restart <service_name>`
* 查看服务状态：`sudo systemctl status <service_name>`
* 查看所有已启用的服务：`sudo systemctl list-unit-files --type=service`
