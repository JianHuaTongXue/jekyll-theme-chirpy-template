---
layout: post
title: Centos用Docker安装V2ray的正确姿势
categories: [V2ray]
tags: [v2ray, Centos, Docker]
date: 2018-02-28 15:15:24.000000000 +08:00
---

# 前言

首先你需要买个vps，并且安装centos的系统。
	
## 一、安装docker


1. 安装 yum-utils，它提供了 yum-config-manager，可用来管理yum源

    `sudo yum install -y yum-utils`

2. 添加docker源

    `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

3. 更新索引

    `sudo yum makecache fast`

4. 安装 docker-ce（社区版）

    `sudo yum -y install docker-ce`

5. 启动 docker

    `sudo systemctl start docker`

6. 验证是否安装成功

	`sudo docker info`或者`sudo docker --version`
	
## 二、安装v2ray docker容器

1. 拉取v2ray docker镜像

    `sudo docker pull v2ray/official`
	
2. 在 /etc 目录下新建一个文件夹 v2ray, 并把你的配置写好后命名为 config.json 放入 v2ray 文件夹内**（ 这一步至关重要 ）**

	> /etc/v2ray/config.json  ，如下：

	```json
	{
		"log": {
			"access": "/var/log/v2ray/access.log",
			"error": "/var/log/v2ray/error.log",
			"loglevel": "warning"
		},
		"inbound": {
			"port": 80,
			"protocol": "vmess",
			"settings": {
				"clients": [{
					"id": "01947a19-d50f-40ad-a3e0-7d25081f82a7",
					"level": 1,
					"alterId": 100
				}]
			},
			"streamSettings": {
				"network": "tcp",
				"tcpSettings": {
					"header": {
						"request": {
							"path": [
								"/"
							],
							"version": "1.1",
							"method": "GET",
							"headers": {
								"Host": "www.baidu.com",
								"Connection": [
									"keep-alive"
								],
								"Accept-Encoding": [
									"gzip, deflate"
								],
								"Pragma": "no-cache",
								"User-Agent": [
									"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.75 Safari/537.36",
									"Mozilla/5.0 (iPhone; CPU iPhone OS 10_0_2 like Mac OS X) AppleWebKit/601.1 (KHTML, like Gecko) CriOS/53.0.2785.109 Mobile/14A456 Safari/601.1.46"
								]
							}
						},
						"type": "http",
						"response": {
							"status": "200",
							"headers": {
								"Transfer-Encoding": [
									"chunked"
								],
								"Connection": [
									"keep-alive"
								],
								"Content-Type": [
									"application/octet-stream",
									"video/mpeg"
								],
								"Pragma": "no-cache"
							},
							"reason": "OK",
							"version": "1.1"
						}
					},
					"connectionReuse": true
				}
			}
		},
		"outbound": {
			"protocol": "freedom",
			"settings": {}
		},
		"outboundDetour": [{
			"protocol": "blackhole",
			"settings": {},
			"tag": "blocked"
		}],
		"routing": {
			"strategy": "rules",
			"settings": {
				"rules": [{
					"type": "field",
					"ip": [
						"0.0.0.0/8",
						"10.0.0.0/8",
						"100.64.0.0/10",
						"127.0.0.0/8",
						"169.254.0.0/16",
						"172.16.0.0/12",
						"192.0.0.0/24",
						"192.0.2.0/24",
						"192.168.0.0/16",
						"198.18.0.0/15",
						"198.51.100.0/24",
						"203.0.113.0/24",
						"::1/128",
						"fc00::/7",
						"fe80::/10"
					],
					"outboundTag": "blocked"
				}]
			}
		}
	}
	```
	
3. 部署v2ray docker容器

	将vps的 /etc/v2ray 文件夹映射到 v2ray docker 容器的 /etc/v2ray 文件夹下

	将vps的 8888 端口映射到 v2ray docker 容器的 80 端口

	`sudo docker run -d --name v2ray -v /etc/v2ray:/etc/v2ray -p 8888:80 v2ray/official v2ray -config=/etc/v2ray/config.json`
	
	键入以上命令后，命令行会出现一串字符，代表容器部署成功。可以立即通过客户端连接并开始使用了，如果不行，则查看 docker log。

	通过以下命令来启动 V2Ray:

	`sudo docker container start v2ray`
	
    停止 V2Ray:

	`sudo docker container stop v2ray`
	
	重启 V2Ray:

	`sudo docker container restart v2ray`
		
	查看日志:

	`sudo docker container logs v2ray`
	
	更新配置后，需要重新部署容器，命令如下：

	1. 先停止运行的容器。
	
		`sudo docker container stop v2ray`
		
	2. 再移除容器 
	
		`sudo docker container rm v2ray`
	
	3. 最后重新部署

		`sudo docker run -d --name v2ray -v /etc/v2ray:/etc/v2ray -p 8888:80 v2ray/official v2ray -config=/etc/v2ray/config.json`
