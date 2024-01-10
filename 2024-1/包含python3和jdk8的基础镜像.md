


```dockerfile
FROM adoptopenjdk/openjdk8:centos

USER root

# System packages
RUN yum clean all && yum update -y && \
    yum install -y wget unzip procps && \
    yum install -y curl gcc zlib zlib-devel libffi libffi-devel openssl openssl-devel && \
    yum clean all && rm -rf /var/cache/yum

# 下载 Python 3.11+ 的源代码
RUN wget https://www.python.org/ftp/python/3.11.6/Python-3.11.6.tgz

# 解压源代码
RUN tar -xzf Python-3.11.6.tgz

# 进入源代码目录
WORKDIR Python-3.11.6

# 编译并安装 Python 3.11+
RUN ./configure --prefix=/usr/local/python3.11 && \
    make && \
    make install

# 创建指向 /usr/local/python3.11/bin/python3 的符号链接 /usr/bin/python
RUN ln -s /usr/local/python3.11/bin/python3 /usr/bin/python3

# 创建指向 /usr/local/python3.11/bin/pip3 的符号链接 /usr/bin/pip
RUN ln -s /usr/local/python3.11/bin/pip3 /usr/bin/pip3

WORKDIR /

RUN rm -rf Python-3.11.6.tgz Python-3.11.6

ENTRYPOINT ["tail", "-f", "/dev/null"]
```