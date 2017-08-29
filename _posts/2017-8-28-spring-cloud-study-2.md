---
layout: post
title:  "spring-cloud 学习笔记(1)"
tags : 
    - java
    - spring-cloud
categories: spring-cloud
---
{:toc}

# STEP 1 : Eureka 微服务的注册与发现
作为spring-cloud中最重要最核心的组件,spring-cloud学习的第一步当然是学习使用Eureka.


首先让我们先了解一下Eureka的背景.

>Eureka是Netflix开源的一款**提供服务注册和发现**的产品，它提供了完整的服务注册和发现。也是spring-cloud体系中最重要最核心的组件之一。

既然是提供**服务注册与发现**,那当然需要有**server(服务注册中心)** 和 **client(服务提供方)** , 那么接下来,我们分别对server和客client进行学习.

## 服务注册中心 (server)
  
在spring-cloud的代码中已经主动帮我们实现了服务注册中心,这样让开发人员只需要非常简单的几个步骤就可以实现服务注册中心的搭建.

### 1.初始化


#### 方法一 : 使用spring initializr进行快速初始化
>[spring initializr](https://start.spring.io/) 能帮助开发人员快速搭建项目的文件结构以及各种依赖.

1. 填写组织名称和项目名称
2. 搜索 ```Eureka Server``` ,并选中
3. Generate Project (即下载项目包)
4. 将项目包解压并导入到IDE中

![](/assets/images/post-images/2017-8-28-spring-cloud-study-2/eureka-server-1.png)

#### 方法二 : 使用IDE自带的工具进行初始化
**此处使用IntelliJ IDEA作为IDE进行演示**

1. 新建spring initializr项目
2. 填写组织名称和项目名称
3. 选中 Cloud Discovery下的Eureka Server

![](/assets/images/post-images/2017-8-28-spring-cloud-study-2/eureka-server-2.png)

![](/assets/images/post-images/2017-8-28-spring-cloud-study-2/eureka-server-3.png)

>在使用spring initializr作为项目初始化工具时,直接选择Eureka Server将默认引入spring-cloud支持而不需要手动引入**spring-cloud-starter**

>如果未使用spring initializr工具,则需要手动引入spring-cloud依赖

{% highlight xml %}
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter</artifactId>
</dependency>
{% endhighlight %}

### 2.配置

在启动代码中添加 ```@EnableEurekaServer``` 注解:

{% highlight java %}

@SpringBootApplication
@EnableEurekaServer
public class SpringCloudEurekaServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringCloudEurekaServerApplication.class, args);
	}
}
{% endhighlight %}

在默认设置下，该服务注册中心也会将自己作为客户端来尝试注册它自己，所以我们需要禁用它的客户端注册行为，在```application.properties```添加以下配置：

{% highlight java %}
spring.application.name=spring-cloud-eureka-server

server.port=9999
eureka.client.register-with-eureka=false 	//表示是否将自己注册到Eureka Server，默认为true
eureka.client.fetch-registry=false		//表示是否从Eureka Server获取注册信息，默认为true。

eureka.client.serviceUrl.defaultZone=http://127.0.0.1:${server.port}/eureka/  
//设置与Eureka Server交互的地址，查询服务和注册服务都需要依赖这个地址。默认是http://localhost:8761/eureka ；多个地址可使用 , 分隔。
{% endhighlight %}

### 3.运行 

配置完成后,运行spring-cloud-eureka-server工程,访问 `http://127.0.0.1:9999/eureka/`,就可以看见以下页面,但是并没有任何服务.

![](/assets/images/post-images/2017-8-28-spring-cloud-study-2/eureka-server-4.png)