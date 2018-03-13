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
