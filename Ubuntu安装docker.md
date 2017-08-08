# 安装docker
```
apt install docker.oi  
```
# 安装docker-compose
```
apt install docker-compose
```
# 克隆docker到本地目录
```
git clone https://github.com/lizhenggan/docker.git
```

# 查看程序安装目录 whereis　程序
```
 whereis docker-compose　
```
# 编译安装nginx镜像 
```
cd docker-compose/nginx

sudo docker build -t nginx:v1.0 .   
```
> 编译nginx镜像tag:v1.0  主要执行Dockerfile 文件 如有需要可重新编辑Dockerfile文件再编译即可

# 编译安装php-fpm镜像
```
cd ../php-fpm5.6

sudo docker build -t php-fpm:v5.6 .  
```
> 编译安装php-fpm镜像 首先进入php-fpm5.6文件夹 再执行编译安装. 编译安装主要执行Dockerfile文件 如需要安装扩展,拷贝文件等编辑Dockerfile再编译安装即可

# 配置docker-compose.yml 
```
cd ../docker-compose/http-server/

>>> 举例配置 可以参考docker-compose.yml 配置语法

phpfpm: 
      image: php-fpm:v5.6                  #镜像名称和版本
      links:                               #在该容器内访问其他容器(可以比喻成域名)
         - mysql
         - redis
      volumes:                             #映射地址 本机地址:容器内地址
          - /var/www:/var/www/html
          - /var/phpDocker:/root/ying
      ports:                               #映射短裤 本机端口:容器内端口
         - "9000:9000"

```
> 进入该目录编辑docker-compose.yml配置文件 

# 新建nginx配置所需文件目录
```
mkdir -p log/nginx/log
```
> nginx映射根目录下（例如/var/www/） 新建log/nginx/log目录

# 配置nginx
```
location ~ \.php$ {
    root           html;
    fastcgi_pass   phpfpm:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  /var/www/html$fastcgi_script_name;
    include        fastcgi_params;
}
```
> /var/www/html 为宿主机项目目录（例如项目目录为shop 则目录为/var/www/html/shop ，文件内的映射目录（root选项）也需要修改为/usr/share/nginx/html/shop）。路由重定向需要参考nginx重定向配置

# 启动docker 
```
docker-compose up -d
```
> docker-compose up -d :可带启动镜像名称 如nginx phpfpm 空格隔开

# 进入docker容器
```
docker-compose exec 容器名称 bash
```
# docker 安装扩展

> 常规安装：修改php-fpm配置文件 重新编译一次php-fpm重启docker即可  

> 容器内部安装如下：

```
 cd /usr/local/bin  
 ./docker-php-ext-install pdo_mysql  
 ```
 
> 在phpfpm容器内部安装扩展。参考网址
http://blog.csdn.net/tinyjian/article/details/55006624

# git自动部署

> 参考 https://github.com/lizhenggan/webhook

# 查看docker容器信息
```
sudo docker inspect docker_redis_1
```
# docker和本地文件传输
```
http://blog.csdn.net/leafage_m/article/details/72082011
```
# 添加docker镜像
```
https://github.com/koolob/swoole-docker-example
```