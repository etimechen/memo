# MQTT 命令笔记

## 主题订阅

`hbmqtt_sub --url mqtt://localhost -t /geektime/iot`

## 主题发布

`hbmqtt_pub --url mqtt://localhost -t /geektime/iot -m Hello,World!`


## EMQX Dashboard 账号密码

用户名：admin

密码：public

## EMQX 启动 Docker

`docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx/emqx:4.2.11`