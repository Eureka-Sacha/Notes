
> debian 的默认jdk包为default-jdk ，版本是11； 但是我需要的是8，所以需要添加源
> 后期如果有多个版本的jdk需要切换，可以使用update-alternatives --config java
```shell

apt-get clean && apt-get update -y 
apt-get install -y apt-transport-https ca-certificates wget dirmngr gnupg software-properties-common 
wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | apt-key add - 
add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/ 
apt-get install -y adoptopenjdk-8-hotspot
java -version


```