# Spring Database 연동

## DB source 설정

### DriverManager

**root-contextDB.xml**

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.shinhan.myapp, net.firstzone"/>

    **<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="locations">
            <list>
                <value>/WEB-INF/spring/appServlet/oracleDB.properties</value>
                <value>classpath:/application.properties</value>
            </list>
        </property>
        <property name="fileEncoding" value="UTF-8"/>
    </bean>**

    <!--DriverManager 이용하기-->
    **<bean id="dataSource"
          class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="${driverClassName}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
    </bean>
</beans>**
```

**oracleDB.properties**

```java
driverClassName=oracle.jdbc.driver.OracleDriver
url=jdbc:oracle:thin:@localhost:1521:FREE
username=hr
password=hrpass
```

### Connection Pooling

의존성 추가

**pom.xml**

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-dbcp2</artifactId>
    <version>2.7.0</version>
</dependency>
```

**root-contextDB.xml**

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="locations">
        <list>
            <value>/WEB-INF/spring/appServlet/oracleDB.properties</value>
            <value>classpath:/application.properties</value>
        </list>
    </property>
    <property name="fileEncoding" value="UTF-8"/>
</bean>

<!--Connection Pooling 이용하기-->
<bean class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${driverClassName}"/>
    <property name="url" value="${url}"/>
    <property name="username" value="${username}"/>
    <property name="password" value="${password}"/>
</bean>
```

### JNDI

**server context.xml**

```xml
<Resource name="jdbc/myoracle" auth="Container"
              type="javax.sql.DataSource" driverClassName="oracle.jdbc.OracleDriver"
              url="jdbc:oracle:thin:192.168.0.35:1521:FREE"
              username="delivery" password="pass1234" maxTotal="20" maxIdle="10"
              maxWaitMillis="-1"/>
```

**root-contextDB.xml**

```xml
<!--JDNI 이용하기-->
<jee:jndi-lookup 
    id="dataSource"
    resource-ref="true"
    jndi-name="jdbc/myoracle"></jee:jndi-lokkup>
```