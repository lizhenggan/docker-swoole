FROM index.alauda.cn/library/php:5-fpm
RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng12-dev \
		&& docker-php-ext-install iconv mcrypt mysqli mysql zip json gd pdo_mysql pcntl \
		&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
		&& pecl install redis \
		&& docker-php-ext-enable redis 


#######################
#容器的启动脚本,运行一个挂起的命令，保持容器不自动退出
		USER	root
		COPY	my_init.sh /sbin/my_init.sh
		RUN		chmod 755 /sbin/my_init.sh

#######################
#每次加载
#ENTRYPOINT	["/sbin/my_init.sh"]
