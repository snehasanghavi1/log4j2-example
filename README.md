
## Introduction
This example illustrates log4j2 application logging. It will create new directory under JBOSS_HOME/bin/ named as logs/ and generated entries will be displayed in app.log.
Refer this official documentation for log4j2 configurations and examples - https://logging.apache.org/log4j/2.x/manual/configuration.html

### Steps to follow :

1. Package a copy of log4j2-*.jar in your deployment.
  ```
    <dependencies>
    ....
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.17.1</version>
   </dependency>
   <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.17.1</version>
   </dependency>
  </dependencies>
```

2. Add the system property -Dorg.jboss.as.logging.per-deployment=false in your configuration file

```
   <system-properties>
        <property name="org.jboss.as.logging.per-deployment" value="false"/>
    </system-properties>
```

3. Create a jboss-deployment-structure.xml to exclude org.apache.logging.log4j.api from the root of your top level deployment in WEB-INF for wars, Here is an example of what you'd put in the WEB-INF folder of an WAR:

```
<jboss-deployment-structure>
  <deployment>
    <exclusions>
      <module name="org.apache.logging.log4j.api" />
    </exclusions>
  </deployment>
</jboss-deployment-structure>
```
4. Add the log4j2.xml in src/main/resources directory

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="warn" name="MyApp">
  <Appenders>
          <Console name="Console" target="SYSTEM_OUT">
                    <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
          </Console>
          <File name="MyFile" fileName="logs/app.log">
                <PatternLayout>
                        <Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
                </PatternLayout>
          </File>
  </Appenders>
  <Loggers>
    <Root level="debug">
      <AppenderRef ref="Console"/>
      <AppenderRef ref="MyFile"/>
    </Root>
  </Loggers>
</Configuration>
```

5. Instantiate logger per class

```
       Logger logger = LogManager.getLogger(GreeterEJB.class);
```   

