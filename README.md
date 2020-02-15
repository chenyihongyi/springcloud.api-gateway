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