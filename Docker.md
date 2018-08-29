# Docker
---
刚刚踩到的一个关于docker exec 的小坑。
原来在运行docker exec 时 command里包含 "&&" 连接字符是无法识别且不会报错的（具体原因有空再查）
根据官方文档，必须要使用sh -c 的形式才可以
```
docker exec -it my_container "echo a && echo b"   #失败
docker exec -it my_container sh -c "echo a && echo b"  #成功
```
>COMMAND should be an executable, a chained or a quoted command will not work. Example: docker exec -ti my_container "echo a && echo b" will not work, but docker exec -ti my_container sh -c "echo a && echo b" will.

---
又碰到了一个关于docker 容器的小坑。
docker 容器启动后，使用
```
docker exec -it my_container bash
```
进入容器，查看编码环境变量
```
echo $LANG
```
发现为`zh_CN.UTF-8` ， 本以为没问题。  
起卡西！！！  这玩意居然完全没起作用！  导致保存中文时全是"?"，  查看/etc/profile 发现并不是UTF-8编码。
删除容器，修改启动参数  `-e LANG="zh_CN.UTF-8"` 后一切正常。
具体的docker启动逻辑没找到现成的文档，留待后面查看吧。  反正下次启动容器有必要的话一定要指定环境变量

---
又踩了一个Docker build 时的小坑。
具体表现为Dockerfile 中添加的
```
RUN yum -y install vim redis-cli;
```
无法执行， 报错如下
```
"Could not resolve host: yum.tbsite.net; Name or service not known"
```
而手动启动镜像缺可以执行。
问了下google大神， 发现报错原因和网络环境有关，启动Docker 时指定DNS服务(--dns) 可修复问题

---
接上个问题，
昨天设置完之后没有充分测试，以为已经OK了。  
其卡西！！！  今天又跑了下docker build 发现完全没有成功！ 还是原来的问题。
问了下google大神又发现了以下文档： 
[官方文档](https://success.docker.com/article/how-do-i-enable-the-remote-api-for-dockerd)
[野生文档](https://www.ivankrizsan.se/2016/05/18/enabling-docker-remote-api-on-ubuntu-16-04/)
简单概括下就是：
>由于我的Docker是本地源码安装所以并不会使用__/etc/default/docker__为启动配置， 所以这个配置相当于白设置了。  所以只能退而求其次。直接覆盖Ubuntu service启动配置。 
