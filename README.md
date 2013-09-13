#Maven GAE Plugin

##Versions
`maven-gae-plugin` released versions.  

<table>
  <thead>
    <tr><th>maven-gae-plugin</th><th>gae-runtime</th></tr>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
    <ul>
       <li>Version 0.9.5</li>
       <li>Version 0.9.4<br/>
         Supports a way of adding arbitrary flags.</li>
       <li>Version 0.9.3<br/>
         Support for --application argument</li>
       <li>Version 0.9.2<br/>
         Code moved to github.</li>
       <li>Version 0.9.1<br/>
         Adds update-dos goal (issue 145).<br/>
         Fixes long standing cron goal issue (issue 117).</li>
       <li>Version 0.9.0<br/>
         Adds Backends Support (issue 135).</li>
       <li>Version 0.8.4<br/>
         Updates to some used maven plugins<br/>
         Fix to the way sourceEncoding is done (issue 120).</li>
       <li>Version 0.8.2<br/>
         Minor bug fixes.</li>
    </ul>
    </td>
    <td valign="top">
    <ul>
       <li><b>Any</b> Version See Configuration Below</li>
       <li>Version 1.8.4</li>
       <li>Version 1.8.3</li>
       <li>Version 1.7.0<br/>
         Targets Google App Engine 1.7.0<br/>
         Updates the datanucleus-appengine dependency to 2.0.1.1</li>
       <li>Version 1.6.4<br/>
         Targets Google App Engine 1.6.4</li>
       <li>Version 1.6.3<br/>
         Targets Google App Engine 1.6.3</li>
       <li>Version 1.6.2.1<br/>
         Targets Google App Engine 1.6.2.1</li>
      </ul>
    </td>
    </tr>
  </tbody>
</table>


##Maven Configuration
* How to setup the `maven-gae-plugin` in the `pom.xml` so that it can use **any** Google App Engine version stated.

  ```xml
  <properties>
      <!-- GAE Plugin ((almost) Any GAE Version) -->
      <gae.version>1.7.7</gae.version>
      <webappDirectory>${project.build.directory}/${project.build.finalName}</webappDirectory>
      <gae.home>${settings.localRepository}/com/google/appengine/appengine-java-sdk/${gae.version}/appengine-java-sdk-${gae.version}</gae.home>
  </properties>
  
  <plugin>
      <groupId>net.kindleit</groupId>
      <artifactId>maven-gae-plugin</artifactId>
      <version>0.9.5</version>
      <configuration>
          <unpackVersion>${gae.version}</unpackVersion>
          <appDir>${webappDirectory}</appDir>
          <sdkDir>${gae.home}</sdkDir>
          
          <!-- Add credentials to ~/.m2/settings.xml <id>appengine-credentials</id> -->
          <serverId>appengine-credentials</serverId>
          <splitJars>true</splitJars>
      </configuration>
      <dependencies>
          <!-- Google App Engine API -->
          <dependency>
              <groupId>com.google.appengine</groupId>
              <artifactId>appengine-api-1.0-sdk</artifactId>
              <version>${gae.version}</version>
          </dependency>
          <!-- Google App Engine Runtime Dependencies -->
          <dependency>
              <groupId>org.apache.geronimo.specs</groupId>
              <artifactId>geronimo-jta_1.1_spec</artifactId>
              <version>1.1.1</version>
              <scope>runtime</scope>
          </dependency>
          <dependency>
              <groupId>org.apache.geronimo.specs</groupId>
              <artifactId>geronimo-jpa_3.0_spec</artifactId>
              <version>1.1.1</version>
              <scope>runtime</scope>
          </dependency>
          <dependency>
              <groupId>javax.jdo</groupId>
              <artifactId>jdo2-api</artifactId>
              <version>2.3-eb</version>
              <scope>runtime</scope>
          </dependency>
          <dependency>
              <groupId>org.datanucleus</groupId>
              <artifactId>datanucleus-core</artifactId>
              <version>1.1.5</version>
          </dependency>
          <dependency>
              <groupId>com.google.appengine.orm</groupId>
              <artifactId>datanucleus-appengine</artifactId>
              <version>1.0.10</version>
              <scope>runtime</scope>
          </dependency>
          <dependency>
              <groupId>org.datanucleus</groupId>
              <artifactId>datanucleus-jpa</artifactId>
              <version>1.1.5</version>
              <scope>runtime</scope>
          </dependency>
          <!-- App Engine Runtime Dependencies -->
          <dependency>
              <groupId>com.google.appengine</groupId>
              <artifactId>appengine-tools-sdk</artifactId>
              <version>${gae.version}</version>
          </dependency>
      </dependencies>
  </plugin>
  ```

* How to setup the plugin using this maven plugin GAE runtime.  

  ```xml
  <properties>
       <gae.version>1.7.7</gae.version>
       <webappDirectory>${project.build.directory}/${project.build.finalName}</webappDirectory>
       <gae.home>
           ${settings.localRepository}/com/google/appengine/appengine-java-sdk/${gae.version}/appengine-java-sdk-${gae.version}
       </gae.home>
  <propertiess>
  
  <plugin>
      <groupId>net.kindleit</groupId>
      <artifactId>maven-gae-plugin</artifactId>
      <version>0.9.5</version>
      <configuration>
          <sdkDir>${gae.home}</sdkDir>
              <!-- Add credentials to ~/.m2/settings.xml <id>appengine-credentials</id> -->
              <serverId>appengine-credentials</serverId>
              <splitJars>true</splitJars>
          </configuration>
          <executions>
              <execution>
                  <id>unpack</id>
                  <phase>validate</phase>
                  <goals>
                      <goal>unpack</goal>
                  </goals>
              </execution>
              <execution>
                  <id>deploy</id>
                  <goals>
                      <goal>deploy</goal>
                  </goals>
              </execution>
          </executions>
      </plugin>
  ```
* Using this plugins latest snapshot add the `pluginRepositories` element to the pom.

  ```xml
  <pluginRepositories>
     <pluginRepository>
       <id>sonatype.snapshots</id>
       <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
     </pluginRepository>
  </pluginRepositories>
  ```

##Maven Configuration for Integration Testing
* Using the GAE Runner for integration tests with Selenium. 

    ```xml
    <properties>
        <!-- GAE Plugin ((almost) Any GAE Version) -->
        <gae.version>1.7.7</gae.version>
        <webappDirectory>${project.build.directory}/${project.build.finalName}</webappDirectory>
        <gae.home>${settings.localRepository}/com/google/appengine/appengine-java-sdk/${gae.version}/appengine-java-sdk-${gae.version}</gae.home>
        
        <!-- These are setup by default. -->
        <gae.wait>false</gae.wait>
        <gae.stop.port>8081</gae.stop.port>
        <gae.port>8888</gae.port>
        <gae.address>127.0.0.1</gae.address>
    </properties>
    
    <plugin>
        <groupId>net.kindleit</groupId>
        <artifactId>maven-gae-plugin</artifactId>
        <!-- Use the snapshot until we get 9.6 released. -->
        <version>0.9.6-SNAPSHOT</version>
        <configuration>
            <unpackVersion>${gae.version}</unpackVersion>
            <appDir>${webappDirectory}</appDir>
            <sdkDir>${gae.home}</sdkDir>
            
            <!-- Add credentials to ~/.m2/settings.xml <id>appengine-credentials</id> -->
            <serverId>appengine-credentials</serverId>
            <splitJars>true</splitJars>
        </configuration>
        <executions>
            <execution>
                <id>unpack</id>
                <phase>validate</phase>
                <goals>
                    <goal>unpack</goal>
                </goals>
            </execution>
            <execution>
                <id>start-gae</id>
                <phase>pre-integration-test</phase>
                <goals>
                    <goal>start</goal>
                </goals>
            </execution>
            <execution>
                <id>stop-gae</id>
                <phase>post-integration-test</phase>
                <goals>
                    <goal>stop</goal>
                </goals>
            </execution>
        </executions>
        <dependencies>
            <!-- Google App Engine API -->
            <dependency>
                <groupId>com.google.appengine</groupId>
                <artifactId>appengine-api-1.0-sdk</artifactId>
                <version>${gae.version}</version>
            </dependency>
            <!-- Google App Engine Runtime Dependencies -->
            <dependency>
                <groupId>org.apache.geronimo.specs</groupId>
                <artifactId>geronimo-jta_1.1_spec</artifactId>
                <version>1.1.1</version>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.geronimo.specs</groupId>
                <artifactId>geronimo-jpa_3.0_spec</artifactId>
                <version>1.1.1</version>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>javax.jdo</groupId>
                <artifactId>jdo2-api</artifactId>
                <version>2.3-eb</version>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>org.datanucleus</groupId>
                <artifactId>datanucleus-core</artifactId>
                <version>1.1.5</version>
            </dependency>
            <dependency>
                <groupId>com.google.appengine.orm</groupId>
                <artifactId>datanucleus-appengine</artifactId>
                <version>1.0.10</version>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>org.datanucleus</groupId>
                <artifactId>datanucleus-jpa</artifactId>
                <version>1.1.5</version>
                <scope>runtime</scope>
            </dependency>
            <!-- App Engine Runtime Dependencies -->
            <dependency>
                <groupId>com.google.appengine</groupId>
                <artifactId>appengine-tools-sdk</artifactId>
                <version>${gae.version}</version>
            </dependency>
        </dependencies>
    </plugin>
    ```

##Maven Generated site information
You can find a copy of the maven generated site information [here](http://sites.kitsd.com/maven-gae-plugin/) and [here](http://maven-gae-plugin.github.com/maven-gae-plugin/).


##Boilerplate / Archetypes
[JAppStart](http://code.google.com/p/jappstart) is a very complete jump start for java GAE developers. [Spring Roo](http://www.springsource.org/roo) is also a great tool for setting up all the boilerplate code.

You can also find the following archetypes for your applications:
 * Plain JSP based example: 

        mvn archetype:generate -DarchetypeGroupId=net.kindleit -DarchetypeArtifactId=gae-archetype-jsp \
        -DarchetypeVersion=0.9.4 -DgroupId=com.myapp.test -DartifactId=testapp

 * Wicket based example

        mvn archetype:generate -DarchetypeGroupId=net.kindleit -DarchetypeArtifactId=gae-archetype-wicket \
        -DarchetypeVersion=0.9.4 -DgroupId=com.myapp.test -DartifactId=testapp

 * GWT based example

        mvn archetype:generate -DarchetypeGroupId=net.kindleit -DarchetypeArtifactId=gae-archetype-gwt \
        -DarchetypeVersion=0.9.4 -DgroupId=com.myapp.test -DartifactId=testapp

 * JSF based example

        mvn archetype:generate -DarchetypeGroupId=net.kindleit -DarchetypeArtifactId=gae-archetype-jsf \
        -DarchetypeVersion=0.9.4 -DgroupId=com.myapp.test -DartifactId=testapp


##Issues
Issues are tracked in [github](https://github.com/maven-gae-plugin/maven-gae-plugin/issues).

##Build Server
* [TeamCity Build Server](http://teamcity.gonevertical.org/project.html?projectId=project12)
