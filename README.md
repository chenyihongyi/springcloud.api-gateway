1、微服务网关介绍和使用场景
	简介：讲解网关的作用和使用场景 (画图)
	
	1）什么是网关
		API Gateway，是系统的唯一对外的入口，介于客户端和服务器端之间的中间层，处理非业务功能 提供路由请求、鉴权、监控、缓存、限流等功能

			统一接入
				智能路由
				AB测试、灰度测试
				负载均衡、容灾处理
				日志埋点（类似Nignx日志）

			流量监控
				限流处理
				服务降级

			安全防护
				鉴权处理
				监控
				机器网络隔离


	2）主流的网关
		zuul：是Netflix开源的微服务网关，和Eureka,Ribbon,Hystrix等组件配合使用，Zuul 2.0比1.0的性能提高很多
		
		kong: 由Mashape公司开源的，基于Nginx的API gateway
		
		nginx+lua：是一个高性能的HTTP和反向代理服务器,lua是脚本语言，让Nginx执行Lua脚本，并且高并发、非阻塞的处理各种请求

2、SpringCloud的网关组件zuul基本使用
	简介：讲解zuul网关基本使用

	1、加入依赖


	2、启动类加入注解 @EnableZuulProxy
		默认集成断路器  @EnableCircuitBreaker

		默认访问规则  
			http://gateway:port/service-id/**

				例子：默认 /order-service/api/v1/order/save?user_id=2&product_id=1
					 自定义 /xdclass_order/api/v1/order/save?user_id=2&product_id=1

		自定义路由转发：
			zuul:
			 routes:
			 	order-service: /apigateway/**


		环境隔离配置：
			需求 ：不想让默认的服务对外暴露接口
				/order-service/api/v1/order/save

			配置：
			zuul: 
				ignored-patterns:
					- /*-service/api/v1/order/save

3、高级篇幅之Zuul常用问题分析和网关过滤器原理分析

	简介：讲解Zuul网关原理和过滤器生命周期，
	 
	1、路由名称定义问题
		路由映射重复覆盖问题
	
	2、Http请求头过滤问题

	3、过滤器执行顺序问题 ，过滤器的order值越小，越先执行
	

	4、共享RequestContext，上下文对象

4、自定义Zuul过滤器实现登录鉴权实战
	简介：自定义Zuul过滤器实现登录鉴权实战

	1、新建一个filter包

	2、新建一个类，实现ZuulFilter，重写里面的方法

	3、在类顶部加注解，@Component,让Spring扫描

5、高级篇幅之高并发情况下接口限流特技	
	简介：谷歌guava框架介绍，网关限流使用

	1、nginx层限流

	2、网关层限流

6、Zuul微服务网关集群搭建
	简介：微服务网关Zull集群搭建

	1、nginx+lvs+keepalive 
	https://www.cnblogs.com/liuyisai/p/5990645.html
	
7.
1、微服务下的链路追踪讲解和重要性
		简介：讲解什么是分布式链路追踪系统，及使用好处
		
	2、SpringCloud的链路追踪组件Sleuth
		简介：讲解分布式链路追踪组件Sleuth实战

		1、官方文档
		http://cloud.spring.io/spring-cloud-static/Finchley.SR1/single/spring-cloud.html#sleuth-adding-project

		2、什么是Sleuth
			一个组件，专门用于记录链路数据的开源组件

			[order-service,96f95a0dd81fe3ab,852ef4cfcdecabf3,false]

			1、第一个值，spring.application.name的值
			
			2、第二个值，96f95a0dd81fe3ab ，sleuth生成的一个ID，叫Trace ID，用来标识一条请求链路，一条请求链路中包含一个Trace ID，多个Span ID
			
			3、第三个值，852ef4cfcdecabf3、spanid 基本的工作单元，获取元数据，如发送一个http

			4、第四个值：false，是否要将该信息输出到zipkin服务中来收集和展示。

		3、添加依赖
			<dependency>
	         <groupId>org.springframework.cloud</groupId>
	         <artifactId>spring-cloud-starter-sleuth</artifactId>
	     	</dependency>






	3、SpringCloud的链路追踪组件Sleuth常见问题说明
		简介：讲解分布式链路追踪组件Sleuth常见问题说明


	  


	4、可视化链路追踪系统Zipkin部署
		简介：讲解Zipkin的介绍和部署
		1、什么是zipkin
			官网：https://zipkin.io/
			大规模分布式系统的APM工具（Application Performance Management）,基于Google Dapper的基础实现，和sleuth结合可以提供可视化web界面分析调用链路耗时情况			
			
		2、同类产品
			鹰眼（EagleEye）
			CAT
			twitter开源zipkin，结合sleuth
			Pinpoint，运用JavaAgent字节码增强技术
			StackDriver Trace (Google) 

		3、开始使用
			https://github.com/openzipkin/zipkin
			https://zipkin.io/pages/quickstart.html

			zipkin组成：Collector、Storage、Restful API、Web UI组成

		4、知识拓展：OpenTracing
		OpenTracing 已进入 CNCF，正在为全球的分布式追踪，提供统一的概念和数据标准。 
		通过提供平台无关、厂商无关的 API，使得开发人员能够方便的添加（或更换）追踪系统的实现。


		推荐阅读：
			http://blog.daocloud.io/cncf-3/
			https://www.zhihu.com/question/27994350
			https://yq.aliyun.com/articles/514488?utm_content=m_43347

	5、高级篇幅之链路追踪组件Zipkin+Sleuth
		简介：使用Zipkin+Sleuth业务分析调用链路分析实战
		
		1、文档
		http://cloud.spring.io/spring-cloud-static/Finchley.SR1/single/spring-cloud.html#_sleuth_with_zipkin_via_http
  		
  		sleuth收集跟踪信息通过http请求发送给zipkin server，zipkinserver进行跟踪信息的存储以及提供Rest API即可，Zipkin UI调用其API接口进行数据展示

  		默认存储是内存，可也用mysql、或者elasticsearch等存储

		2、加入依赖
		<dependency>
		    <groupId>org.springframework.cloud</groupId>
		    <artifactId>spring-cloud-starter-zipkin</artifactId>
		</dependency>

		里面包含 spring-cloud-starter-sleuth、spring-cloud-sleuth-zipkin

		3、文档说明：http://cloud.spring.io/spring-cloud-static/Finchley.SR1/single/spring-cloud.html#_features_2

		4、配置zipkin.base-url

		5、配置采样百分闭spring.sleuth.sampler


		推荐资料：
			https://blog.csdn.net/jrn1012/article/details/77837710