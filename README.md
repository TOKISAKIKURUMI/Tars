# **目录**

- 概述
- 开发环境搭建说明
- API接口说明
- API接口使用样例

# **概述**

  为分布式名字发现服务提供java客户端jar包，提供查询服务节点信息，模调数据上报等功能接口。

# **开发环境搭建说明**

#### **环境依赖**

- JDK1.7或以上版本
- Maven2.2.1或以上版本

   JDK与maven的安装方法请自行查找，不在这里介绍。

#### **构建工程**

通过IDE或是命令行创建一个maven项目，这里以eclipse为例，File -> New -> Project -> Maven Project -> maven-archetype-webapp(或是archetype-quickstart,以自己的实际需求为准)，生成一个maven项目。

#### 依赖配置

在刚才创建的项目的pom.xml中添加seer依赖

```xml
<dependency>
  	<groupId>qq-cloud-central</groupId>
  	<artifactId>tars-seer</artifactId>
  	<version>1.0.1</version>
</dependency>
```
# API接口说明

**1.API key的申请**

首先需要为自己的业务在管理页面上申请一个key, 该key将作为初始化API的参数，为API设置key的方法如下。

```
RouterConfig.setSeerApiKey("your server's API key");  //填入自己业务集的真实API key
```



**2.接口说明**

| 编号   | 接口或类的名称       | 功能说明                                     |
| ---- | :------------ | ---------------------------------------- |
| 1    | RouterFactory | 实例化路由接口的工厂类，单例类，可以分别使用Agent方式和纯api方式两种方式实例化路由接口。 |
| 2    | RouterConfig  | 名字路由的配置类。                                |
| 3    | Router        | 名字路由对外提供的接口，可用于查询服务节点，上报模调数据。            |

**3.接口方法说明**

**3.1初始化路由接口**

| 方法原型                        | 参数说明 | 返回值              | 所属类                 |
| --------------------------- | ---- | ---------------- | ------------------- |
| Router createAgentRouter(); | 无参数  | Router的Agent方式实例 | RouterFactory(静态方法) |
| Router createApiRouter();   | 无参数  | Router的纯api方式实例  | RouterFactory(静态方法) |

**3.2获取单个服务节点**

| 概述   | 按照指定方式获取服务的单个节点                          |
| :--- | ---------------------------------------- |
| 方法原型 | Result<RouterResponse> getRouter(RouterRequest request); |
| 参数说明 | request: 入参，需要获取服务的相关信息，主要包括请求服务名，获取方式，负载均衡方式等。 |
| 返回值  | 返回值中包括返回码和返回路由信息(RouterResponse)，返回码为0表示成功，-1表示失败，失败时会包含返回的错误信息。 |
| 备注   | RouterRequest类结构和RouterResponse类结构见下方代码。 |



RouterRequest:

```java
public class RouterRequest {

    private String obj; //请求服务名称，必填
    private LBGetType lbGetType; //按IDC、set或all获取，默认ALL,选填
    private String setInfo; //set信息，当按set方式获取时必填
    private LBType lbType; //负载均衡类型，默认轮询，选填
    private long hashKey; //hash key，选择哈希负载均衡时必填
    private String callModuleName; //主调模块名，模调数据上报时需要该信息，若为空则用本地Ip替代，可选（推荐填写）
    ....省略getter和setter方法
}
```



LBGetType:

```java
public enum LBGetType {
    LB_GET_IDC(0), //按IDC
    LB_GET_SET(1), //按set
    LB_GET_ALL(2); //全部获取
}
```



LBType:

```java
public enum LBType {
    LB_TYPE_LOOP(0), //轮询
    LB_TYPE_RANDOM(1), //随机
    LB_TYPE_STATIC_WEIGHT(2), //静态权重
    LB_TYPE_CST_HASH(3), //一致性哈希
    LB_TYPE_ALL(4);  //获取所有节点，调用getRouter方法时不要设置成此类型,否则默认返回列表中的第一个节点
}
```



RouterResponse:

```java
public class RouterResponse {
	
	private ServerNode serverNode; //返回的服务节点
	private String responseMsg;   //返回信息
	...
}
```



ServerNode:

```java
public class ServerNode {
	private String sIP;    //服务的IP
	private int port;    //服务端口
	private int timeout; //节点访问超时时间
	private boolean bTcp;  //是否是tcp通信
	...
}
```



**3.3获取服务的节点列表**

| 概述   | 按照指定方式获取服务的所有节点                          |
| ---- | ---------------------------------------- |
| 方法原型 | Result<RoutersResponse> getRouters(RoutersRequest request); |
| 参数说明 | request: 入参，需要获取服务的相关信息，主要包括请求服务名，获取方式。  |
| 返回值  | 返回值中包括返回码和返回路由信息(RoutersResponse)，返回码为0表示成功，-1表示失败，失败时会包含返回的错误信息 |
| 备注   | RoutersRequest类结构和RoutersResponse类结构见下方代码。 |

RoutersRequest:

```java
public class RoutersRequest {
	
	private String obj;
	private LBGetType lbGetType;
	private String setInfo;
	...
}
```

RoutersResponse:

```java
public class RoutersResponse {
	
	private String responseMsg;
	private List<ServerNode> node;    //返回节点列表
	...
}
```

**3.4上报模调数据**

|      |      |
| ---- | ---- |
|      |      |
|      |      |
|      |      |

