FROM centos:centos6

# update
RUN yum -y update

# timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# epel
RUN rpm --import http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-6
RUN rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# remi
RUN rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

# user
RUN useradd app
RUN yum -y install sudo
RUN echo "app ALL=(ALL) ALL" > /etc/sudoers.d/app
RUN echo 'app:app' | chpasswd
RUN echo "export LANG=ja_JP.UTF-8" >> /home/app/.bashrc

# ssh
RUN yum -y install openssh-server
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config
RUN /etc/init.d/sshd start

# nginx
RUN yum -y install nginx

# supervisor
RUN yum -y install python-setuptools
RUN easy_install pip
RUN pip install supervisor

RUN mkdir /etc/supervisord.d
RUN mkdir /var/log/supervisor
ADD ./etc/supervisord.conf /etc/supervisord.conf

ADD ./etc/supervisord.d/sshd.ini /etc/supervisord.d/sshd.ini
ADD ./etc/supervisord.d/nginx.ini /etc/supervisord.d/nginx.ini
