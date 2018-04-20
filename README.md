# hazelcast-tomcat-sessionmanager-42-example

See: https://github.com/hazelcast/hazelcast-tomcat-sessionmanager/issues/42

This repo reproduces the issue linked above using:
 
* an nginx docker container running as a round-robin load balancer 
* three basic tomcat instances upstream from the nginx instance, configured to use the hazelcast-tomcat-sessionmanager
* basic gatling tests executing concurrent requests to simulate concurrent ajax requests and reproduce the issue.

#### Prerequisites
* Maven 3+
* Docker (i.e. https://docs.docker.com/docker-for-mac/install/)


#### Setup the Hazelcast Session Manager:

```
mkdir -p cluster/lib
curl http://central.maven.org/maven2/com/hazelcast/hazelcast-tomcat7-sessionmanager/1.1.3/hazelcast-tomcat7-sessionmanager-1.1.3.jar > cluster/lib/hazelcast-tomcat7-sessionmanager-1.1.3.jar
curl http://central.maven.org/maven2/com/hazelcast/hazelcast-all/3.9.1/hazelcast-all-3.9.1.jar > cluster/lib/hazelcast-all-3.9.1.jar
mvn clean package
```

#### Start the docker container 
```
docker-compose up
```

#### Run the gatling tests
In a different terminal window:
```
cd gatling-test
mvn gatling:execute
```

The location of the gatling reports will be printed to the terminal at the end of execution

#### Results
The results can be viewed in the gatling report. 
 
Further the following message (apart from the session id) can be seen in the docker-compose logs.
```
tomcatTwo_1    | INFO: No Session found for:1857EF2BEE65A74635C23BDC98E78B11
```

