FROM centos8-yum:repo2

# solve please select the geographic area in which you live problems
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

ADD ./docker/sources.list /etc/apt/sources.list
ADD ./docker/pip.conf /root/.pip/pip.conf
ADD ./docker/docker-entrypoint.sh /tmp/docker-entrypoint.sh

# RUN gzip /etc/yum.repos.d/CentOS-* 
# COPY CentOS-Base.repo /etc/yum.repos.d/

# ADD ./docker/CentOS-Linux-BaseOS.repo  /etc/yum.repos.d/CentOS-Linux-BaseOS.repo
# ADD ./docker/aliyun.repo /etc/yum.repos.d/aliyun.repo

# COPY ./docker/CentOS-Linux-BaseOS.repo /etc/yum.repos.d/CentOS-Linux-BaseOS.repo

# RUN wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo
# RUN mv CentOS-Base.repo  CentOS-Linux-BaseOS.repo
# RUN sed -i -e 's/failovermethod/#failovermethod/g' /etc/yum.repos.d/CentOS-Linux-BaseOS.repo

RUN yum clean all
RUN yum makecache
RUN yum repolist
RUN yum update -y
RUN yum install -y git
RUN yum install -y python38
RUN yum install -y python3-pip
RUN yum install -y epel-release && yum install -y nodejs npm
RUN cd /opt && git clone https://gitee.com/zhujf21st/authbase.git && cd authbase
RUN cd ui && npm install && cd ..
RUN pip3 install -r requirements.txt
RUN mv /tmp/docker-entrypoint.sh /opt/authbase/docker-entrypoint.sh && chmod +x /opt/authbase/docker-entrypoint.sh
RUN yum install -y mysql-server
RUN systemctl start mysqld && cd /opt/authbase &&  mysql < db.sql || true && mysql < init-user.sql
RUN yum remove -y git && yum clean all

WORKDIR /opt/authbase
ENTRYPOINT ["sh", "docker-entrypoint.sh"]