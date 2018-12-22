## jobx

一个功能完善通用的 Linux 定时任务调度定系统，满足多场景下复杂的定时任务调度，同时集成了 Linux 实时监控、webssh，提供一个方便管理定时任务的平台。

## 升级日志

V1.2.0 by 2018-08-27

1. 支持 Windows 端，实现全平台编译和安装。
2. 简化安装部署流程，提供三个脚本一键安装启动项目(build.sh,agent.sh,server.sh)。
3. 更换项目 rpc 调度框架 thrift 为 Netty 和 Mina。
4. 引入 zookeeper，更改心跳检测机制。
5. 基于 zookeeper，agent 实现自动注册。
6. 基于 zookeeper 实现 server 集群，动态增减 server, job自动分配, 实现高可用。
7. server 端支持单机和集群两种部署方式，对用户来说一个参数决定哪种部署方式。
8. 可以自由定义任务的执行身份。
9. server 内置 tomcat 和 jetty 组件，支持开发者模式，大大减轻运维部署的难度。
10. webssh 新增基于 privateKey 方式的登录。
11. 支持上传文件到 agent。
12. 更强大的任务流调度。
13. 支持对外提供 api 的方式，允许通过接口调度任务。
14. 新增一系列的 examples 调度使用实例。
15. 修复一系列已知bug，增强稳定性和可用性。

**此版本是里程碑版本,从底层架构到项目实现都进行重新规划,大大提高了可用性和稳定性,建议所有人升级**

V1.1.0 by 2017-04-20

1. 新增 agent 和 server 代理连接方式。
2. 新增 quartz 类型的任务调度。
3. 新增 web 终端，实现 web 终端文件上传，主题更换。
4. 更改 agent 和 server 心跳检查机制。
5. 新增流程任务调度。
6. 增加 xss 和 csrf 注入攻击的预防。
7. server 端增加踢人下线单一登录控制。

## 功能特色

你是否在执行定时任务时常遇到下面问题。

 - 需要在每台 linux 服务器的 crontab 里一一定义任务。
 - 任务的执行监控不方便。
 - 需要登录到每台机器查看定时任务的运行结果，机器一多简直是一种灾难。
 - 多台机器协同处理一个任务很麻烦，如何保证多台机器上的任务按顺序依次执行?
 - 任务运行失败，重新执行需要再次定义执行时间重跑，重跑完成了还得改回正常时间。
 - 正在运行的任务要 kill 掉很麻烦。
 
jobx 将彻底的解决上面所有问题，主要功能如下：

 -  自动化管理任务，提供可操作的 web 图形化管理。
 -  要当场执行只需点击执行即可，非常方便。
 -  时间规则支持 quartz 和 crontab，更强大更灵活。
 -  非常方便的修改任务的执行时间。
 -  任务的运行状态实时查看。
 -  支持任务 kill (包括由当前任务调起的其他子任务链，彻底 kill)。
 -  支持重新执行正在运行的任务。
 -  出错后实时通知给任务人(超过重跑次数自动发送邮件，短信)。
 -  支持任务超时设置，一旦超过预定运行时长自动 kill，任务结束，防止僵尸任务。
 -  支持流程任务(多台机器上协同完成一个大的任务, 按任务分配的顺序依次执行每台机器上的任务)。
 -  记录任务的运行日志，非常方便查看。
 -  多用户多角色。
 -  现场执行(选择 N 台机器同时执行一个命令或任务)。
 -  webssh，浏览器一键 ssh 登录到 linux 服务器。
 -  提供服务器的性能实时监控。

## 运行环境

- Java JDK 1.7 or greater
  http://www.oracle.com/technetwork/java/javase/overview/index.html

- Tomcat server 8.0 or greater
  https://tomcat.apache.org

- Zookeeper
  http://zookeeper.apache.org/

- Redis
  https://redis.io/

- Browser IE10+

   
## 安装说明

- jobx 分为 jobx-server 端和 jobx-agent 端。
- jobx-server 为一个 web 可视化的中央管理调度平台。
- jobx-agent 为要管理的任务的机器, agent 和 server 都依赖 zookeeper, 安装部署 jobx 之前必须先安装和启动 zookeeper。
- server 和 agent必须连接同一个 zookeeper, server端依赖 redis。

## 编译步骤:

```
1)：下载源码: 

> git clone https://github.com/jobxhub/jobx.git

2)：编译:

# 进入deployment

> cd deployment
  
# 修改配置信息
jobx.password=jobx
jobx.port=1577
jobx.registry=zookeeper://${zookeeper_host}:2181?bakup=${zookeeper_host1}:2181,${zookeeper_host2}:2181

#jobx.host=
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/jobx
jdbc.username=root
jdbc.password=${password}

jobx.cluster=false
jobx.cached=memcached

redis.host=${redis.host}
redis.password=${redis.password}
redis.port=6379

memcached.servers=${memcached.servers}
memcached.protocol=BINARY

# *nix平台执行 sh build.sh
# window平台双击 build.bat
    
3)：启动

1） agent
    *nix平台:  执行 sh agent.sh
    window平台: 需要进入 jobx-agent/target 下，解包 jobx-agent-${version}.tar.gz，双击bin/startup.bat

2)  server
    *nix平台:  执行 sh server.sh 8080
    window平台 双击 server.bat

3)  进入到 jobx 的管理端，如果agent也启动了，应该可以直接在 server 的执行器页面看到 agent，则添加任务即可。
```  

## 注意事项:

```
1)：如果自行编译项目的，有可能agent端的脚本执行失败，这时请更改agent/bin下所有的脚本的字符集。
   a) vim *.sh
   b) :set ff=unix 保存退出即可
   
2)：如果 agent 已经成功启动 server 还是连接不上，请检查 agent 端口是否开放(很多云服务器得开放端口才能访问)。

3)：如果server端用nginx做反向代理，配置如下：

   
upstream jobx {
     server 127.0.0.1:8080;
}

server {
    listen 80;
    server_name www.jobx.org;
    root /data/www/jobx/;

    location / {
        proxy_pass        http://jobx;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        client_max_body_size  10m;
        client_body_buffer_size 1m;
        proxy_connect_timeout 300;
        proxy_send_timeout    300;
        proxy_read_timeout    300;
        proxy_buffer_size     4k;
        proxy_buffers    4   32k;
        proxy_busy_buffers_size 64k;
        proxy_temp_file_write_size  64k;
    }

    #这里必须这么配置,否则web终端无法使用
    location  ^~  /terminal.ws {
        proxy_pass http://jobx;
        proxy_redirect    off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

}
```

## 常见问题:

1. Q: 创建作业运行身份无法选择？

   A: 由于考虑到权限的问题，当前登录的用户不能随便指定任务的执行身份，需要超级管理员权限的用户(jobx)登录，在设置页面统一设置运行身份,
   多个运行身份用","分割，然后超级管理员再编辑用户，为该用户指定可以执行的身份(可选择多个)
   这样用户在创建任务的时候就可以选择指定身份去执行了

更多问题请加入 jobx 交流群156429713，欢迎大家加入。
    
