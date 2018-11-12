```
什么叫消息队列
消息（Message）是指在应用间传送的数据。消息可以非常简单，比如只包含文本字符串，也可以更复杂，可能包含嵌入对象。

消息队列（Message Queue）是一种应用间的通信方式，消息发送后可以立即返回，由消息系统来确保消息的可靠传递。消息发布者只管把消息发布到 MQ 中而不用管谁来取，消息使用者只管从 MQ 中取消息而不管是谁发布的。这样发布者和使用者都不用知道对方的存在。
```
RabbitMQ 中交换器主要分为四种类型：direct、fanout、topic 以及 headers，headers 用的比较少，就不讲了。
* direct
>根据路由键全文匹配去寻找绑定到此交换器上的匹配成功的队列，然后投递消息。

* fanout
>把消息投递给所有绑定到此交换器上的队列，而且会忽略路由键。

* topic
根据路由键通配符匹配去寻找绑定到此交换器上的匹配成功的队列，然后投递消息

```java
@Profile("direct")
```
在 RabbitMQ 中，有两种 acknowledgement 模式
* 自动 acknowledgement 模式
> 发后即忘模式

* 手动 acknowledgement 模式

ReturnCallback 比 ConfirmCallback 先回调

```
RabbitMQ 是一个由 Erlang 语言开发的 AMQP 的开源实现。

AMQP ：Advanced Message Queue，高级消息队列协议。它是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件可传递消息，并不受产品、开发语言等条件的限制。


1. 可靠性（Reliability），如持久化、传输确认、发布确认。

2. 灵活的路由（Flexible Routing）
在消息进入队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了一些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在一起，也通过插件机制实现自己的 Exchange 。

3. 消息集群（Clustering）
多个 RabbitMQ 服务器可以组成一个集群，形成一个逻辑 Broker 。

优点:
高可用（Highly Available Queues）
队列可以在集群中的机器上进行镜像，使得在部分节点出问题的情况下队列仍然可用。

多种协议（Multi-protocol）
RabbitMQ 支持多种消息队列协议，比如 STOMP、MQTT 等等。

多语言客户端（Many Clients）
RabbitMQ 几乎支持所有常用语言，比如 Java、.NET、Ruby 等等。

管理界面（Management UI）
RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。

跟踪机制（Tracing）
如果消息异常，RabbitMQ 提供了消息跟踪机制，使用者可以找出发生了什么。

插件机制（Plugin System）RabbitMQ 提供了许多插件，来从多方面进行扩展，也可以编写自己的插件。


systemctl status rabbitmq-server

关闭远程节点
rabbitmqctl -n rabbit@server.example.com stop 

rabbitmqctl list_queues
rabbitmqctl reset

rabbitmqctl stop_app
rabbitmqctl start_app
list_exchanges name type durable auto_delete
list_bindings

rabbitmq-plugins list
rabbitmq-plugins enable rabbitmq_management

http://127.0.0.1:15672/
输入默认账号: guest   密码: guest

添加用户
rabbitmqctl add_user mytest mytest
rabbitmqctl set_user_tags mytest administrator
rabbitmqctl set_permissions -p / mytest '.*' '.*' '.*'
rabbitmqctl list_permissions

```
