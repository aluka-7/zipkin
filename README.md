# Zipkin是什么
Zipkin分布式跟踪系统；它可以帮助收集时间数据，解决在microservice架构下的延迟问题；它管理这些数据的收集和查找；Zipkin的设计是基于谷歌的Google Dapper论文。

每个应用程序向Zipkin报告定时数据，Zipkin UI呈现了一个依赖图表来展示多少跟踪请求经过了每个应用程序；如果想解决延迟问题，可以过滤或者排序所有的跟踪请求，并且可以查看每个跟踪请求占总跟踪时间的百分比。

# 为什么使用Zipkin

随着业务越来越复杂，系统也随之进行各种拆分，特别是随着微服务架构和容器技术的兴起，看似简单的一个应用，后台可能有几十个甚至几百个服务在支撑；一个前端的请求可能需要多次的服务调用最后才能完成；当请求变慢或者不可用时，我们无法得知是哪个后台服务引起的，这时就需要解决如何快速定位服务故障点，Zipkin分布式跟踪系统就能很好的解决这样的问题。

# Zipkin下载和启动

官方提供了三种方式来启动，这里使用第二种方式来启动;

```shell script
wget -O zipkin.jar 'https://search.maven.org/remote_content?g=io.zipkin.java&a=zipkin-server&v=LATEST&c=exec'
java -jar zipkin.jar

```

首先下载zipkin.jar，然后直接使用-jar命令运行，要求jdk8以上版本；

```shell script 
[root@localhost ~]# java -jar zipkin.jar 
                                    ********
                                  **        **
                                 *            *
                                **            **
                                **            **
                                 **          **
                                  **        **
                                    ********
                                      ****
                                      ****
        ****                          ****
     ******                           ****                                 ***
  ****************************************************************************
    *******                           ****                                 ***
        ****                          ****
                                       **
                                       **
 
 
             *****      **     *****     ** **       **     **   **
               **       **     **  *     ***         **     **** **
              **        **     *****     ****        **     **  ***
             ******     **     **        **  **      **     **   **
 
:: Powered by Spring Boot ::         (v1.5.8.RELEASE)
......
on || application/json]}" onto public java.lang.Object org.springframework.boot.actuate.endpoint.mvc.HealthMvcEndpoint.invoke(javax.servlet.http.HttpServletRequest,java.security.Principal)
2017-12-06 22:09:17.498  INFO 7555 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2017-12-06 22:09:17.505  INFO 7555 --- [           main] o.s.c.support.DefaultLifecycleProcessor  : Starting beans in phase 0
2017-12-06 22:09:17.789  INFO 7555 --- [           main] b.c.e.u.UndertowEmbeddedServletContainer : Undertow started on port(s) 9411 (http)
2017-12-06 22:09:17.794  INFO 7555 --- [           main] zipkin.server.ZipkinServer               : Started ZipkinServer in 16.867 seconds (JVM running for 19.199)
```

基于Undertow WEB服务器，提供对外端口:9411，可以打开浏览器访问[http://ip:9411](http://localhost:9411)

# Zipkin架构
跟踪器(Tracer)位于你的应用程序中，并记录发生的操作的时间和元数据,提供了相应的类库，对用户的使用来说是透明的，收集的跟踪数据称为Span;将数据发送到Zipkin的仪器化应用程序中的组件称为Reporter,Reporter通过几种传输方式之一将追踪数据发送到Zipkin收集器(collector)，然后将跟踪数据进行存储(storage),由API查询存储以向UI提供数据。

# 如何使用

```go
zipkin.Init([]trace.Tag{})
```
