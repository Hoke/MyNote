# docker 实战

- [docker 实战](#docker-实战)
  - [镜像的导入和导出](#镜像的导入和导出)
  - [启动 docker 失败 Got permission denied while trying to connect to the Docker daemon socket](#启动-docker-失败-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket)
  - [从 docker 私有仓库拉取镜像失败 http: server gave HTTP response to HTTPS client](#从-docker-私有仓库拉取镜像失败-http-server-gave-http-response-to-https-client)

## 镜像的导入和导出

```sh
# 拉取镜像到本地
docker pull myregistrydomain.com:5000/myserver.v1.0.0
# 查看拉取到镜像的 image id
dokcer images
# 保存镜像到文件:或 docker save -o myserver.v1.0.0.tar image_id
docker save image_id > myserver.v1.0.0.tar
# 通过各种方式传输压缩文件到需要使用的机器
# 导入镜像:或 docker load -i myserver.v1.0.0.tar
docker load < myserver.v1.0.0.tar
# 查看导入的镜像，有 image id (和上面保存时的一样) 但是 repository 和 tag 为空
docker images
# 标记镜像，将其归入某一仓库
docker tag image_id myregistrydomain.com:5000/myserver.v1.0.0
```

## 启动 docker 失败 Got permission denied while trying to connect to the Docker daemon socket

```sh
kiki@ubuntu:~$ docker version
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        2d0083d
 Built:             Fri Aug 16 14:19:38 2019
 OS/Arch:           linux/amd64
 Experimental:      false
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.39/version: dial unix /var/run/docker.sock: connect: permission denied
# 原因：docker 进程使用 Unix Socket 而不是 TCP 端口。而默认情况下，Unix socket 属于 root 用户，因此需要 root 权限才能访问
# 添加 docker 用户组
kiki@ubuntu:~$ sudo groupadd docker
[sudo] password for kiki:
groupadd: group 'docker' already exists
# 检测当前用户是否已经在 docker 用户组中
kiki@ubuntu:~$ sudo gpasswd -a kiki docker
Adding user kiki to group docker
# 更新 docker 用户组
kiki@ubuntu:~$ newgrp docker
# ok
kiki@ubuntu:~$ docker version
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        2d0083d
 Built:             Fri Aug 16 14:19:38 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.09.7
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       2d0083d
  Built:            Thu Aug 15 15:12:41 2019
  OS/Arch:          linux/amd64
  Experimental:     false
```

## 从 docker 私有仓库拉取镜像失败 http: server gave HTTP response to HTTPS client

```sh
kiki@ubuntu:~$ docker pull myregistrydomain.com:5000/image_name
Error response from daemon: Get https://myregistrydomain.com:5000/v2/: http: server gave HTTP response to HTTPS client
# 参考 https://github.com/docker/docker.github.io/blob/master/registry/insecure.md 设置 http 连接
# 修改或创建 /etc/docker/daemon.json
# 添加 { "insecure-registries":["myregistrydomain.com:5000"] }
kiki@ubuntu:~$ sudo vim /etc/docker/daemon.json
# 重启 docker 服务
kiki@ubuntu:~$ sudo service docker restart
kiki@ubuntu:~$ docker pull myregistrydomain.com:5000/image_name
# ok
```
