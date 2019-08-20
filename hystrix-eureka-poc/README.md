# Hystrix-Eureka with SpringCloud by Netflix

### Presentation

**[Slides](docs/Slides.pdf)**


### POC
This POC covers the following features by SpringCloud NetFlix Stack:

Features  | Using
------------- | -------------
Each Springboot application registered as Eureka client | [https://spring.io/guides/gs/service-registration-and-discovery]
Hystrix Circuit-breakers and their fallback methods to potentially-failing method calls in each **sample** service | [https://spring.io/guides/gs/circuit-breaker]
Out-of-the-box Hystrix Dashboard for monitoring services health and traffics and Eureka Dashboard for inspecting the registered instances | [https://github.com/Netflix/Hystrix/wiki/Dashboard]
Remote service **simulator** to simulate remote service availability and unavailability |
Polling application to poll sample service endpoints that depend on remote service |


### Get sample Springboot applications up and running in order

<pre>
$ cd ../eureka-server
$ mvn spring-boot:run

$ cd ../hystrix-prototype
$ mvn spring-boot:run

$ cd ../remote-service-simulator
$ mvn spring-boot:run

$ cd ../polling-service
$ mvn spring-boot:run

$ cd ../hystrix-dashboard
$ mvn spring-boot:run
</pre>


### Access the dashboards

Open browsers and navigate to

Dashboards  | Test URLs
------------- | -------------
Eureka Server dashboard to view registration status | [http://localhost:8761/](http://localhost:8761/)
Hystrix dashboard to watch the traffics and metrics | [http://localhost:8081/hystrix/monitor?stream=http://localhost:8082/turbine.stream](http://localhost:8081/hystrix/monitor?stream=http://localhost:8082/turbine.stream)
Turbine stream | [http://localhost:8082/turbine.stream/](http://localhost:8082/turbine.stream)


### Access the dashboards

* Start off with all sample services up and running, and the remote service call in IAMServiceClient being functional, therefore the circuit breakers stay
closed.
![Circuits closed](docs/hystrix_dashboard1.png)

* Bring the remote service IAMService down by killing off (Ctrl-C) the remote-service-simulator

* Watch it make the circuit trip and color of volume circle changing from green to yellow, then to orange and finally to red; status changing from Closed
 to Open. Most of services were made up to implement with FailFast fallback except AssetService.getAssetsAPI with FailSilent fallback
![Circuits trip](docs/hystrix_dashboard2.png)

* Bring the remote service IAMService back up by running remote-service-simulator again

* Watch the circuits start turning green again and statuses changing from Open to Closed