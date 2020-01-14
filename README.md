# eclipse

enviroment : Spring boot

## How to update latest Code

~~~

* root folder -> right click -> Team -> Update to HEAD

* root folder -> right click -> Team -> Revert

* Main menu -> Project -> Clean -> Project Click

* 12 + Network -> disable cache 하면 캐시 없이 바로 로딩됨 ㅋㅋㅋ

~~~

## Using the SDK for Java 1.x and 2.x Side by Side

pom.xml

~~~

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.hist</groupId>
	<artifactId>macle</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>macle</name>
	<description>-</description>
	<properties>
		<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.1.1</version>
		</dependency>
		<dependency>
	         <groupId>io.springfox</groupId>
	         <artifactId>springfox-swagger2</artifactId>
	         <version>2.9.2</version> 
	     </dependency>	     
	     <dependency>
	         <groupId>io.springfox</groupId>
	         <artifactId>springfox-swagger-ui</artifactId>
	         <version>2.9.2</version>
	     </dependency>     
		<dependency>
			<groupId>org.springframework.session</groupId>
			<artifactId>spring-session-core</artifactId>
		</dependency>
		<!-- Add Hikari Connection Pooling support -->
		<dependency>
			<groupId>com.zaxxer</groupId>
			<artifactId>HikariCP</artifactId>
		</dependency>		
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<dependency>
		    <groupId>software.amazon.awssdk</groupId>
		    <artifactId>aws-sdk-java</artifactId>
	      	<version>2.10.29</version>
	      	<scope>compile</scope>
	    </dependency>
		<dependency>
      		<groupId>com.amazonaws</groupId>
      		<artifactId>aws-java-sdk-ec2</artifactId>
      		<version>1.11.245</version>
    	</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-engine</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-junit-jupiter</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>


~~~

* you have to concentrate on 

~~~
<dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-ec2</artifactId>
    <version>1.11.245</version>
</dependency>

~~~~
