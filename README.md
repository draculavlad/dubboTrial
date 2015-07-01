# dubboTrial
* try dubbo

## references ##
* https://github.com/alibaba/dubbo/tree/dubbo-2.4.11
* https://github.com/alibaba/dubbo/issues/22

## in case you need to upload jar to nexus##
```shell
curl -v -u admin:admin123 --upload-file dubbo-demo-api-2.4.11.jar http://173.39.194.55:8081/nexus/content/repositories/thirdparty/com/alibaba/dubbo-demo-api/2.4.11/dubbo-demo-api-2.4.11.jar
```

## setup java dev env ##
* https://raw.githubusercontent.com/draculavlad/JavaDevEnv/master/README.md

## add mvn executing opts for more memory usage ##
```shell
vi $MAVEN_HOME/bin/mvn
    -edit:
        (following the commented area for "Maven2 Start Up Batch script")
        (append the content below)
        MAVEN_OPTS="$MAVEN_OPTS -Xms256m -Xmx1024m -XX:MaxPermSize=128m -XX:ReservedCodeCacheSize=64m"
```

## install zookeeper ##
* https://raw.githubusercontent.com/draculavlad/InstallZookeeper/master/README.md


## download opensesame pom and upload it to your nexus ##
```shell
wget https://raw.githubusercontent.com/alibaba/opensesame/master/pom.xml -O pom.xml
```

## download other dependency jar file and upload it to your nexus ##
* some essential jar file included is fastjson, hessian-lite
* things must be concerned is that
* you have to insert the specific groupId, artifactId, version matching with the sample pom.xml
```shell
wget http://usc.googlecode.com/svn/files/package/alibaba-dubbo-dependency.zip
unzip alibaba-dubbo-dependency.zip
```

## git clone dubbo 2.4.11 ##
```shell
git clone https://github.com/alibaba/dubbo.git dubbo
cd dubbo && git checkout dubbo-2.4.11
```

## update the dubbo/pom.xml ##
* I provided a sample pom.xml content below\
* https://raw.githubusercontent.com/draculavlad/dubboTrial/pom.xml.sample

## build dubbo ##
```shell
cd /opt/dubbo
mvn clean install -Dmaven.test.skip
cd dubbo/target
ls
```

## configure dubbo-demo and dubbo-simple ##
```shell
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-demo/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-demo/dubbo-demo-api/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-demo/dubbo-demo-consumer/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-demo/dubbo-demo-provider/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-simple/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-simple/dubbo-monitor-simple/pom.xml && \
sed -i 's|<version>2.4.10</version>|<version>2.4.11</version>|g' dubbo-simple/dubbo-registry-simple/pom.xml
```
```shell
vi dubbo-demo/dubbo-demo-provider/src/main/assembly/conf/dubbo.properties
    -edit: enable zookeeper as registry
vi dubbo-demo/dubbo-demo-consumer/src/main/assembly/conf/dubbo.properties
    -edit: enable zookeeper as registry

## build and upload dubbo-demo ##
```shell
cd $DUBBO_HOME
cd dubbo-demo
mvn clean install -DskipTests

```
## run consumer and provider##
```shell
cd dubbo-demo/dubbo-demo-provider/target
tar zxf dubbo-demo-provider-2.4.11-assembly.tar.gz
cd dubbo-demo-provider-2.4.11/bin
./start.sh
```
and
```shell
cd dubbo-demo/dubbo-demo-consumer/target
tar zxf dubbo-demo-consumer-2.4.11-assembly.tar.gz
cd dubbo-demo-consumer-2.4.11/bin
./start.sh
```

## run monitorer
```shell
cd dubbo-simple/dubbo-monitor-simple/target/
tar zxf dubbo-monitor-simple-2.4.11-assembly.tar.gz
cd dubbo-monitor-simple-2.4.11/bin
./start.sh




