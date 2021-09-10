# naming-server
naming-server for part of Currency Application currency-exchange-service , currency-conversion-service , git-localconfig-repo and spring-cloud-config-server , naming-server

Eureka - Step 19 to 21
If you see an error of this kind - Wait for 5 minutes and give it a try again!

com.netflix.client.ClientException: Load balancer does not have available server for client: 
(0) Give these settings a try individually in application.properties of all microservices (currency-exchange, currency-conversion, api-gateway) to see if they help

eureka.instance.prefer-ip-address=true
OR

eureka.instance.hostname=localhost
(1) Ensure @EnableEurekaServer is enabled on NetflixEurekaNamingServerApplication

(2) spring-cloud-starter-netflix-eureka-client dependency is added in both the client application pom.xml files.

(3) eureka.client.service-url.default-zone=http://localhost:8761/eureka is configured in application.properties of both currency-exchange-service and currency-conversion-service

(4) Ensure that both the services are registered with Eureka at http://localhost:8761/

(5) Ensure that you are using the right url - http://localhost:8100/currency-conversion-feign/from/USD/to/INR/quantity/10

(6) Ensure that you are able to hit the urls directly - http://localhost:8000/currency-exchange/from/USD/to/INR and http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10

(8) Try if it works when you include the following property in application.properties for currency-conversion-service and currency-exchange-service

eureka.instance.hostname=localhost
(9) Compare code against the complete list of components below (Step 19 to Step 21).

If everything is fine

(1) Make sure you start the services in this order (a)netflix-eureka-naming-server (b)currency-exchange-service (c)currency-conversion-service

(2) Make sure all the components are registered with naming server.

(3) Give a minute of warm up time!

(4) If you get an error once, execute it again after a few minutes

If you still have a problem, post a question including all the details:

(1) Screenshot of services registration with Eureka

(2) Responses from all 3 URLs - http://localhost:8100/currency-conversion-feign/from/USD/to/INR/quantity/10, http://localhost:8000/currency-exchange/from/EUR/to/INR and http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10

(3) Start up logs of the each of the components to understand what's happening in the background!

(4) What was the last working state of the application? Explain in Detail.

(5) Post the version of Spring Boot and Spring Cloud You are Using.

(6) Code for all the components listed below (Step 19 to Step 21):

Step 19
Step 19 - Understand Naming Server and Setting up Eureka Naming Server

Eureka

http://localhost:8761/
/naming-server/src/main/java/com/hanil/microservices/namingserver/NamingServerApplication.java Modified
Add @EnableEurekaServer
package com.hanil.microservices.namingserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@EnableEurekaServer
@SpringBootApplication
public class NamingServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(NamingServerApplication.class, args);
	}

}
/naming-server/src/main/resources/application.properties Modified
spring.application.name=naming-server
server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
Step 20
Step 20 - Connect Currency Conversion Microservice & Currency Exchange Microservice to Eureka

/currency-conversion-service/src/main/resources/application.properties Modified
New Lines

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka
/currency-conversion-service/pom.xml Modified
New Lines

		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
/currency-exchange-service/pom.xml Modified
New Lines

		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
/currency-exchange-service/src/main/resources/application.properties Modified
New Lines

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka