# MaxScaleTree
MaxScale技术研究

<pre>
MaxScale是Maridb开发的一个MySql数据中间件，配置好MySql的主从复制架构后，希望实现读写分离，把读操作分散到从服务器中，并且对多个服务器实现负载均衡。

MaxScale是插件式结构，允许用户可开发适合自己的插件
</pre>

![](https://i.imgur.com/VCszrTO.png)


![](https://i.imgur.com/WfvP1do.png)


第一种方式

![](https://i.imgur.com/8Tkcxuc.png)

第二种方式

![](https://i.imgur.com/gMyhH7q.png)

<pre>
MaxScale实现读写分类的两种方式：
      1）基于Connect类，类似于Haproxy，不解析SQL，可以通过PHP Yii框架或Java Mybatis
         框架实现。在此方式中，用MaxScale做多台Slave的负载均衡，并且支持主从同步延迟检测
         功能。

      2）另一种是基于statement的，要解析SQL语句。
         在这种方式里，前端程序不需要修改，通过MaxScale对SQL语句进行解析，把读/写请求自动
         路由到后端的数据库节点上，从而实现读写分离。商业软件OneProxy中间件也是基
         于statement方式实现读/写分离。这种方式的好处是不修改程序代码，减少了复杂度，可平
         滑迁移，无感知，缺点是解析SQL势必会增加CPU的性能损耗，性能没有基于connect的方式好。
</pre>

<pre>
二、MaxScale的基础组成

    MaxScale 目前提供的插件功能分为5类：

    认证插件
    提供了登录认证功能，MaxScale 会读取并缓存数据库中 user 表中的信息，当有连接进来时，先从缓存信息中进行验证，如果没有此用户，会从后端数据库中更新信息，再次进行验证

    协议插件
    包括客户端连接协议，和连接数据库的协议

    路由插件 
    决定如何把客户端的请求转发给后端数据库服务器，读写分离和负载均衡的功能就是由这个模块实现的

    监控插件
    对各个数据库服务器进行监控，例如发现某个数据库服务器响应很慢，那么就不向其转发请求了

    日志和过滤插件
    提供简单的数据库防火墙功能，可以对SQL进行过滤和容错
</pre>
