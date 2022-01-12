# 创建于2022/01/12
目前这个项目在作为本人的毕业设计中开发，待答辩毕业后，将会将整个项目源码上传
预计上传时间为2022/07/01



# ERP后台管理系统

## 技术选型

~~~text
https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E
~~~

1. 开发环境工具
    * IDEA
    * JDK 1.8
    * Mysql 5.7
    * Redis 6.0.16
    * RabbitMQ 
2. 微服务相关
    * Spring Boot 2.2.5.RELEASE
    * Spring Cloud Hoxton.SR3
    * Spring Cloud Alibaba 2.2.1.RELEASE
    * Nacos 1.1.4
    * Sentinel
        * 目前遇到的问题
            1. sentinel-dashboard 1.7.2
                ~~~text
                # 服务只能连接本地搭建的sentinel图形化界面
                
                要想连接远程界面，需要：
                1.服务所在时间与sentinel-dashboard服务所在时间相同
                2.两个服务器相互ping的通

                好在这只是个图形化界面，配置服务限流等功能并不依赖它
                建议通过配置持久化的方式注入限流等功能
 
                ~~~
        * 配置持久化
            ~~~property
            #第一步，引入pom
            <dependency>
                    <groupId>com.alibaba.csp</groupId>
                    <artifactId>sentinel-datasource-nacos</artifactId>
            </dependency>
            
            #第二步，修改yml
            spring:
              cloud:
                sentinel:
                  datasource:
                    ds1:
                      nacos:
                        server-addr: nacos服务器IP:端口
                        dataId: 项目名称
                        groupId: DEFAULT_GROUP
                        data-type: json
                        rule-type: flow
            
            #第三步，nacos添加配置
            [
                {
                    "resource": "xxx/yyy",
                    "limitApp": "default",
                    "grade": 1,
                    "count": 1,
                    "strategy": 0,
                    "controlBehavior": 0,
                    "clusterMode": false
                }
            ]
            
            # resource 资源名称，最好不要和接口名称完全相同
            # limitApp 来源应用
            # grade 阈值类型 0：线程数 1：QPS
            # count 单机阈值
            # strategy 流控模式 0：直接 1：关联 2：链路
            # controlBehavior 流控效果 0：快速失败 1：Warm Up 2:排队等待
            # clusterMode 是否集群
            
            #调用接口待懒加载加载后即可
            
            ~~~
        
    * Seata
    * openfeign
3. 前端页面
    * Vue2.x
    * ElementUI
4. 测试
    * Junit
    * JMeter
5. 运维
    * 阿里云服务器
    * CentOS 7
    * Docker
 
 
    
## 项目介绍
1. 模块
    * 仓库模块
    * 客户模块
    * 订单模块
    * 财政模块
    * 用户管理模块
2. 权限
    * 超级管理员
    * 各模块管理员
    * 操作员
