https://www.youtube.com/watch?v=q3ZOY-bmXzM

---------------------------------------------
1. odpalamy nexusa:
c:\devtools\nexus\nexus-2.15.1-02\bin\
#nexus install
#nexus start

logwanie: http://localhost:9081/nexus
admin / admin123

tworzymy dwa repozytoria:
- maven-snapshots
- maven-release

----------------------------------------------
2. dytujemy plik settle.xml w katalogu mvn'a:

<servers>
	<server>
      <id>mavendeployment</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
</servers>


	<profiles>
		<profile>
    		<id>snapshots</id>
    		 <repositories>
    			<repository>
    				<id>maven-snapshots</id>
    				<name>your custom repo</name>
    				<url>http://localhost:9081/nexus/content/repositories/maven-snapshots</url>
    			</repository>
    		 </repositories>
    	</profile>

    	<profile>
    		<id>release</id>
    		 <repositories>
    			<repository>
    				<id>maven-release</id>
    				<name>your custom repo</name>
    				<url>http://localhost:9081/nexus/content/repositories/maven-release</url>
    			</repository>
    		 </repositories>
    	</profile>
   </profiles>

----------------------------------------------
3. pom.xml

	<distributionManagement>
		<snapshotRepository>
			<id>mavendeployment</id>
			<url>http://localhost:9081/nexus/content/repositories/maven-snapshots</url>
		</snapshotRepository>
		<repository>
			<id>mavendeployment</id>
			<url>http://localhost:9081/nexus/content/repositories/maven-release</url>
		</repository>
	</distributionManagement>



--------------------------------------------------------

nvn clean package
mnv deploy
lub:
mvn clean deploy -P release


ciągnie z lokalnego nexusa :)
[...]
[INFO] --- maven-deploy-plugin:2.8.2:deploy (default-deploy) @ demo-devops ---
Downloading from mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/maven-metadata.xml
Downloaded from mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/maven-metadata.xml (772 B at 4.8 kB/s)
Uploading to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/demo-devops-0.0.1-20221105.170814-9.war
Uploaded to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/demo-devops-0.0.1-20221105.170814-9.war (8.4 MB at 11 MB/s)
Uploading to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/demo-devops-0.0.1-20221105.170814-9.pom
Uploaded to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/demo-devops-0.0.1-20221105.170814-9.pom (2.2 kB at 9.1 kB/s)
Downloading from mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/maven-metadata.xml
Downloaded from mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/maven-metadata.xml (282 B at 4.5 kB/s)
Uploading to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/maven-metadata.xml
Uploaded to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/0.0.1-SNAPSHOT/maven-metadata.xml (772 B at 13 kB/s)
Uploading to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/maven-metadata.xml
Uploaded to mavendeployment: http://localhost:9081/nexus/content/repositories/maven-snapshots/com/example/demo-devops/maven-metadata.xml (282 B at 1.8 kB/s)[...]


nasz war z aplikacji demo-devops pojawił się w nexus w repozytoriach: Snapshots,maven-snapshots i maven-release
http://localhost:9081/nexus/#view-repositories;snapshots~browseindex

<dependency>
  <groupId>com.example</groupId>
  <artifactId>demo-devops</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <type>war</type>
</dependency>

