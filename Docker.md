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
