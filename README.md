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

Library

~~~

import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.services.ec2.AmazonEC2;
import com.amazonaws.services.ec2.AmazonEC2Client;
import com.amazonaws.services.ec2.model.DescribeInstancesRequest;
import com.amazonaws.services.ec2.model.DescribeInstancesResult;
import com.amazonaws.services.ec2.model.Instance;
import com.amazonaws.services.ec2.model.Reservation;
import com.hist.macle.common.ApiResponseMessage;
import com.hist.macle.service.UsageService;
import com.hist.macle.vo.CategoryCostVo;
import com.hist.macle.vo.MonthlyCostVo;
import com.hist.macle.vo.MonthlyUsageVo;
import com.hist.macle.vo.UserVo;

BasicAWSCredentials awsCreds = new BasicAWSCredentials("", "");
    	
        final AmazonEC2 ec2 = AmazonEC2Client.builder()
        	    .withRegion("ap-northeast-2")
        	    .withCredentials(new AWSStaticCredentialsProvider(awsCreds))
        	    .build();
        
        boolean done = false;

        DescribeInstancesRequest request = new DescribeInstancesRequest();
        
        while(!done) {
            DescribeInstancesResult response = ec2.describeInstances(request);
           
            for(Reservation reservation : response.getReservations()) {

        for(Instance instance : reservation.getInstances()) {
        		 System.out.print(instance.getTags() + " ");
        		 System.out.print(instance.getPrivateIpAddress() + " ");
        		 System.out.println(instance.getPublicDnsName() + " ");
        		 System.out.print(instance.getPlacement() +  " ");
        		 System.out.println(instance.getInstanceId() + " ");
        		 System.out.print(instance.getState() + " ");
                 System.out.print(instance.getSecurityGroups() + " ");
                 System.out.println(instance.getImageId() + " ");
                 System.out.print(instance.getInstanceType() + " ");
                 System.out.print(instance.getArchitecture() + " ");
                 System.out.print(instance.getPublicIpAddress() + " ");
                 System.out.println(instance.getLaunchTime() + " ");
                 System.out.println("---------------------------------------------------------");
                }    
               
            }

            request.setNextToken(response.getNextToken());

            if(response.getNextToken() == null) {
                done = true;
            }
        }

~~~


## Spring concept 

IOC(inversion of control)

흐름의 역전 -> 기존의 개발자가 코드로 흐름을 제어하는게 -> 스프링의 autowied bean 이런걸로 제어의 흐름을 프레임워크 본인이 가지게 됨

DI(Dependency Injection)

그에따라 스프링만의 방식으로 bean 이나 setter등에서 코드의 의존성을 주입

## 스프링 흐름

-> js 파일에서 예를들어 acct값을 parma에 집ㅈ어넣고   ajax 방식으로 data 부분 으로 넘긴다 

->  mapper 인터페이스에서 사용할 @Parmam으로 mapper 폴더 진짜 db 쿼리문에서 작동하고 -> 그오른쪽 변루소 get 함수에서 받아온다

-> 그다음 serviceImpl에서 return 값으로 만든 vo 를 설정하고 받는다,

-> Controller에서 get 값으로 받는다 .

## 오늘 한 실수 

-> JS파일에서 data : param 으로 넘겼는데 바로 아래줄에 data : null이 있엇음

-> 내가 불러 오려고 했던 AK, SK 하필이면 데이터가 없는 곳에 값을 찍고 있엇음


## How to apply ajax url(include Parameter)

if your URL contains parameter(ex : account)

~~~
localhost/awsresource?account=dhtkdals321
~~~


Letme,,,,,,

## How to dynamic row add using Datables(JS Library

if you are add table row using just Html id, it is not appling paging

But if you want to apply paging you have to using Datatables fucntion,,

~~~
exexex
~~~
