FROM ubuntu:14.04
MAINTAINER re-enter <admin@re-enter.com>

RUN if [ ! -d "/opt" ]; then mkdir /opt; fi
RUN if [ ! -d "/tmp" ]; then mkdir /tmp; fi
RUN if [ ! -d "/srv/jboss/config" ]; then mkdir -p /srv/jboss/config; fi

# create jboss folders
RUN mkdir -p /var/log/jboss

# install Oracle JDK: you need to download it manually in the installers directory
ADD ./installers/jdk-7u71-linux-x64.tar.gz /opt/
ENV JAVA_HOME=/opt/jdk1.7.0_71
ENV PATH=${JAVA_HOME}/bin:${PATH}

# install jboss: you need to download JBOSS manually in the installers directory
ADD ./installers/jboss-eap-6.1.0.zip /tmp/
WORKDIR /opt/
RUN apt-get install unzip
RUN unzip /tmp/jboss-eap-6.1.0.zip > /dev/null 2>&1

ENV JBOSS_HOME=/opt/jboss-eap-6.1

# create jboss user
RUN useradd -s /bin/bash -d ${JBOSS_HOME} jboss && chown -R jboss:jboss ${JBOSS_HOME}

# distribute mysql lib
RUN if [ ! -d "${JBOSS_HOME}/modules/com/mysql/main" ]; then mkdir -p ${JBOSS_HOME}/modules/com/mysql/main; fi
ADD ./installers/mysql-connector-java-5.1.15-bin.jar ${JBOSS_HOME}/modules/com/mysql/main/
ADD ./config/modules/com/mysql/module.xml ${JBOSS_HOME}/modules/com/mysql/main/

# distrbute jboss standalone configuration
ADD ./config/configuration/standalone.xml ${JBOSS_HOME}/standalone/configuration/
ADD ./config/configuration/mgmt-users.properties ${JBOSS_HOME}/standalone/configuration/

# distribute scripts
RUN if [ ! -d "/opt/scripts" ]; then mkdir /opt/scripts; fi
ADD ./scripts/jboss.sh /opt/scripts/jboss.sh
RUN chmod -R u+x /opt/scripts/*.sh

# the JBOSS deployments folder is a mounted folder
RUN rm ${JBOSS_HOME}/standalone/deployments/*
VOLUME ["${JBOSS_HOME}/standalone/deployments", "/srv/jboss/config", "/var/log/jboss"]

EXPOSE 8080
EXPOSE 9990
EXPOSE 9012

# docker build  --force-rm=true -t reenter/ubuntu-jboss-eap-6.1 .
# SLM
# docker run -d -p 8080:8080 -p 9990:9990 -p 9012:9012 -h atg_front_slm --name atg_front_slm -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/slm:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/slm:/var/log/jboss --link database:database  reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start

# docker run -d -p 8180:8080 --name atg_front1 -h atg_front1 -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/front1:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/front1:/var/log/jboss --link database:database --link atg_front_slm:atg_front_slm reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start

# docker run -d -p 8280:8080 --name atg_front2 -h atg_front2 -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/front2:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/front2:/var/log/jboss --link database:database --link atg_front_slm:atg_front_slm reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start