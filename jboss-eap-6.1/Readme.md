#JBOSS AS7 container#

##Build##

To build this container, you need to download the following installers in the **installers** directory:

* JBOSS EAP 6.1.0 installer
* Oracle JDK 7 
* mysql JDBC driver (any supported 5 version of your choice)

# Building the container

 sudo docker build --force-rm=true -t reenter/ubuntu-jboss-eap-6.1 .

# running the container

## SLM

```bash
docker run -d -p 8080:8080 -p 9990:9990 -p 9012:9012 -h atg_front_slm --name atg_front_slm -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/slm:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/slm:/var/log/jboss --link database:database  reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start
```

## Other instances

```bash
docker run -d -p 8180:8080 --name atg_front1 -h atg_front1 -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/front1:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/front1:/var/log/jboss --link database:database --link atg_front_slm:atg_front_slm reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start

docker run -d -p 8280:8080 --name atg_front2 -h atg_front2 -v /home/vrakoton/docker-workspace/runtime/atg/config:/srv/jboss/config -v /home/vrakoton/docker-workspace/runtime/atg/ear/front2:/opt/jboss-eap-6.1/standalone/deployments -v /home/vrakoton/docker-workspace/runtime/atg/log/front2:/var/log/jboss --link database:database --link atg_front_slm:atg_front_slm reenter/ubuntu-jboss-eap-6.1 /opt/scripts/jboss.sh start
```