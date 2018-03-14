- 当服务器外网速度慢时可以使用以下方式拷贝文件

在要源服务器的当前目录执行

```
	python -m SimpleHTTPServer
```
默认开启8000端口

在目的服务器上执行

```
	wget http://hostname:8000/要拷贝的文件
```
---

- 批量删除镜像
 
 ```
 docker rmi $(docker images | grep dev | awk '{print $3}')
 
 ```