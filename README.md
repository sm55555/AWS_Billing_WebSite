# AWS_Billing_Website

enviroment : Eclipse ,Spring boot

## How to update latest Code

~~~

* root folder -> right click -> Team -> Update to HEAD

* root folder -> right click -> Team -> Revert

* Main menu -> Project -> Clean -> Project Click

* 12 + Network -> Checking "disable cache" loads immediately without cache

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

Generally, developers control the flow of code -> In Spring developers get control of the flow of code using Autowide, bean

DI(Dependency Injection)

In Spring, Typically Inject the dependency of the code using a bean file or Setter Keyward

## The flow of Spring

-> In Js, Transfer The variable using ajax and call Declared URL(In Controller)

~~~

var getCategoryCost = function(categoryType, acct) {
		if(acct == undefined || acct == null) {
			acct = $("#acct_category").val();
		}
		var param = {
			categoryType : categoryType,
			acct : acct
		};
		
		$.ajax({
		    url:'/usage/category', // 요청 할 주소
		    async:true,// false 일 경우 동기 요청으로 변경
		    type:'GET',
		    data: param,// 전송할 데이터
		    dataType:'json',// xml, json, script, html


~~~

-> In Mapper Interface, Declares the variable in @param and brings the required data from DB using DB query

-> Data is received as object by ORM method and setting by get, set fucntion

-> In serviceImpl, Data is received VO(DataType)

-> In Controller, Data is received using get function and you check Declared URL

## A Mistake made today 

-> In JS section

i normally add parmeter (data : param) but,,,, The nextline of code was initializing parameter (data : null)

~~~

var getCostTrendline = function(acct) {
	if(acct == undefined || acct == null) {
		acct = $("#acct_trendline").val();
	}
	var param = {
		acct : acct
	};

	$.ajax({
	    url:'/usage/trendline', // 요청 할 주소
	    async:true,// false 일 경우 동기 요청으로 변경
	    type:'GET',
	    data: param,// 전송할 데이터
	    data: null, // null data
	    dataType:'json',// xml, json, script, html

~~~

The data i want to get is null,,,,,,




## How to apply ajax url(include Parameter)

if your URL contains parameter(ex : account)

~~~
localhost/awsresource?account=dhtkdals321
~~~


## How to dynamic row add using Datables(JS Library)

if you are add table row using just Html id, it is not appling paging

But if you want to apply paging you have to using Datatables fucntion,,

~~~

for(var i=0; i<rds_Running+rds_Stopped; i++){
	rds_table.row.add( [
		obj.rds_detail[i*4],
		obj.rds_detail[i*4],
		obj.rds_detail[i*4+1],
		obj.rds_detail[i*4+2],
		obj.rds_detail[i*4+3]

    ] ).draw( false );
}

~~~


List로 string string List<Integer> 받기 존나 어ㅕㅂㄷ ㅌㅇ

라이브러리르 만든 객체는 html에서 컨트롤 하는게 아니라 js에서 컨트롤 해야한다..
	
## chart height setting

if you want to modify something Not modify 

~~~
document.getElementById("rds-lineChart").style.height = "40px";
~~~

# 개빡치는 chartjs old data 삭제하는법

첫번쨰로는 그냥 destroy();인스턴스 하려고 했는데 안됬다;;;

그래서 여러 삽질을 하다가 

innertHTML 방식으로 해결

```javascript
document.getElementById("chartContainer").innerHTML = '&nbsp;';
	                		document.getElementById("chartContainer").innerHTML = '<canvas id="datatransfer-lineChart" height="40"></canvas>';
	                		var datatransfer_chart = document.getElementById("datatransfer-lineChart").getContext("2d");
	                		
	                		document.getElementById('datatransfer-lineChart').style.height = "40px";
			                document.getElementById("ec2-lineChart").style.height = "40px";	
			                document.getElementById("rds-lineChart").style.height = "40px";
			                document.getElementById("dynamodb-lineChart").style.height = "40px";
			                
	                		new Chart(datatransfer_chart, {
			                    type : 'line',
			                    data : datatransfer_chart_data,
			                    options : lineOption
	                		});

```

# 개빡치는 datatable head 라인만 왼쪽으로 치우치는 현상

해당 테이블을 다시 그려줘야한다..  $(테이블아이디).dataTable().fnDraw();

<div>
	<img width="800px" height="300px" src="https://user-images.githubusercontent.com/38831314/78088937-b85ac580-7400-11ea-95f0-d10823d639a9.PNG">
</div>

# aws sdk 권한 정상적으로 주기 위한 user, permmision 세팅

user 초기 세팅

<div>
	<img width="800px" height="300px" src="https://user-images.githubusercontent.com/38831314/78106160-ca9f2880-742d-11ea-90e5-d112d2bd3a7c.PNG">
</div>

IAM 권한 관련

<div>
	<img width="800px" height="300px" src="https://user-images.githubusercontent.com/38831314/78105902-3634c600-742d-11ea-9015-9f71d2fdf1e1.PNG">
</div>


# 날짜 전날로 바꾸는거

```javascript

var nowMonthDateList = function() {
                var list = [];
                var date = new Date();
                if(date.getDate() == 1) {
                        date.setDate(0);
                }
                
                do {
                        var current = date.getDate();
                        var month = date.getMonth() + 1;
                        
                        list.unshift(lpad(month, 2, 0) + "-" + lpad(current, 2, 0));
                        current --;
                        date.setDate(current);
                } while(current > 0);
                return list;
        }

```

# 만약 현재가 1일이면 쿼리에서 그전달 1일에서 ~끝일까지 뽑아내는거

```mysql

CURDATE() -> IF(DATE_FORMAT(CURDATE(), '%d') = '01', CURDATE() - INTERVAL 1 DAY, CURDATE())

```


# canvas 화면 모바일로 할때 겹쳐지는거 수정

```html

<div id="ec2-resource-chart-div" class="row" style="display: none;margin-bottom:1rem;height:350px;"></div>

감싸고 있는거 height 충분하게준다.

```

```javascript

$('#ec2-resource-chart-div').html('<canvas id="ec2-lineChart" height="30"></canvas>');

스크립트 이너 html 수정

maintainAspectRatio: false,

차트생성 lineoption 추가 

```



# query 짜르기

기존 SUBSTR(A.USAGE_TYPE, 1, 4) -> A.REGION
