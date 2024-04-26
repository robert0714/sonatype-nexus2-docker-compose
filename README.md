# Nexus 3 OSS Kubernetes Operator
* source code: https://github.com/m88i/nexus-operator
* operator.io:  https://operatorhub.io/operator/nexus-operator-m88i

# sonatype-nexus3-docker-compose
* docker-compose.yml refer: `https://hub.docker.com/r/sonatype/nexus3/`
```yaml
version: "3.3"
services:
   nexus2:
     image: sonatype/nexus3:3.67.1-java8
     container_name: nexus3
     restart: always
     volumes:
       - $PWD/nexus-data:/nexus-data
     ports:
        - "8081:8081" 
     environment:
       - MAX_HEAP=1g 
       - NEXUS_CONTEXT=nexus
       - INSTALL4J_ADD_VM_PARAMS=-Xms2703m -Xmx2703m -XX:MaxDirectMemorySize=2703m
       - TZ=Asia/Taipei
```
* Copy the file ``${project}/settings.xml`` to your ``${your Home Directory}/.m2/settings.xml`` 
```xml
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties> 
            </properties>
            <repositories>
				<repository>
				   <id>nexus3</id>
				   <url>http://localhost:8081/nexus/content/groups/public/</url>
				   <releases>
					  <enabled>true</enabled>
				   </releases>
				   <snapshots>
					  <enabled>true</enabled>
				   </snapshots>
				</repository>
			 </repositories>
			 <pluginRepositories>
				<pluginRepository>
				   <id>nexus3</id>
				   <url>http://localhost:8081/nexus/content/groups/public/</url>
				   <releases>
					  <enabled>true</enabled>
				   </releases>
				   <snapshots>
					  <enabled>true</enabled>
				   </snapshots>
				</pluginRepository>
			 </pluginRepositories>
        </profile>
     </profiles>
     <mirrors>
             <mirror>
                <id>nexus3</id>
                <mirrorOf>external:http:*</mirrorOf>
                <url>http://localhost:8081/nexus/content/groups/public/</url>
                <blocked>false</blocked>
            </mirror> 
      </mirrors> 
    <servers>
      <server>
        <id>nexus3</id>
        <username>admin</username>
        <password>admin123</password>
     </server>
    </servers>
</settings>
```
* when specific url
```bash
mvn clean deploy:deplo  -DaltDeploymentRepository=nexus3::default::http://localhost:8081/nexus/repository/maven-snapshots/
```
# sonatype-nexus2-docker-compose
* Copy the file ``${project}/settings.xml`` to your ``${your Home Directory}/.m2/settings.xml`` 
```xml
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- Optional URL to server. Default value is http://localhost:9000 -->
                <sonar.host.url>http://192.168.62.37/sonar</sonar.host.url>
            </properties>
            <repositories>
				<repository>
				   <id>nexus2</id>
				   <url>http://192.168.62.37/nexus/content/groups/public/</url>
				   <releases>
					  <enabled>true</enabled>
				   </releases>
				   <snapshots>
					  <enabled>true</enabled>
				   </snapshots>
				</repository>
			 </repositories>
			 <pluginRepositories>
				<pluginRepository>
				   <id>nexus2</id>
				   <url>http://192.168.62.37/nexus/content/groups/public/</url>
				   <releases>
					  <enabled>true</enabled>
				   </releases>
				   <snapshots>
					  <enabled>true</enabled>
				   </snapshots>
				</pluginRepository>
			 </pluginRepositories>
        </profile>
     </profiles>
     <mirrors>
             <mirror>
                <id>nexus2</id>
                <mirrorOf>external:http:*</mirrorOf>
                <url>http://192.168.62.37/nexus/content/groups/public/</url>
                <blocked>false</blocked>
            </mirror> 
      </mirrors> 
    <servers>
      <server>
        <id>nexus2</id>
        <username>admin</username>
        <password>admin123</password>
     </server>
    </servers>
</settings>
```
* when specific url
```bash
mvn clean deploy -DaltDeploymentRepository=nexus2::default::http://192.168.62.37/nexus/content/repositories/snapshots/
```
