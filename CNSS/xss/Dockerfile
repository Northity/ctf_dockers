FROM ubuntu:14.04

MAINTAINER N0rth3ty <northity.com>
ENV REFRESHED_AT 2018-09-30
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND noninteractive

# 先写 修改源/更新 【如果必须的话】
# 替换源（这里可用sed或者直接COPY一个完整的sources.list来替换）
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.163.com\/ubuntu\//g' /etc/apt/sources.list
# 进行更新 安装wget
RUN apt update -y && \
	apt install wget -y

# 安装chrome浏览器
RUN apt -y install apt-transport-https
RUN wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/ 
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | apt-key add - 
RUN apt update -y
RUN apt install -y google-chrome-stable


# 安装apache2 php5 php扩展
RUN apt -yqq install apache2 php5 libapache2-mod-php5 \
	php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick \
	php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell \
	php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
# 安装php5
#RUN apt -yqq install php5 libapache2-mod-php5
# 安装php扩展
#RUN apt install -yqq php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
#安装python及必须组件
RUN apt install -y python2.7 python-pip git
RUN pip install --upgrade pip && \
	pip install selenium setuptools --upgrade
#RUN pip install selenium setuptools
#RUN git clone https://github.com/urllib3/urllib3.git
#WORKDIR ./urllib3
#RUN python setup.py install
#RUN pip install urllib3


# 再 修改一些配置
# 对于文件中的替换字符串，多使用sed命令
# 例如去掉apache2的列目录漏洞

RUN sed -i 's/Options Indexes FollowSymLinks/Options None/' /etc/apache2/apache2.conf

# 添加文件到相应目录
ADD html/ /var/www/html/
ADD xssbot.py /var/www/xssbot.py
# ADD geckodriver /usr/bin/geckodriver
ADD chromedriver /var/www/chromedriver

#这里的ip需要替换为实际使用的ip和端口
RUN sed -i "s/http:\/\/localhost/http:\/\/x.x.x.x:2000/g" /var/www/xssbot.py
# 修改为可执行
# RUN chmod 777 /usr/bin/geckodriver
RUN chmod 777 /var/www/chromedriver

# 修改目录权限为可写
RUN chmod 777 -R /var/www/html/post
RUN chmod 777 -R /var/www/html/submit_fdasfh2jfka

# 删除Apache主页
RUN rm /var/www/html/index.html
RUN echo "ServerName localhost:80" >> /etc/apache2/apache2.conf

# 添加start.sh，并准备开机执行
COPY ./start.sh /root/start.sh
RUN chmod +x /root/start.sh
ENTRYPOINT cd /root; ./start.sh

EXPOSE 80

