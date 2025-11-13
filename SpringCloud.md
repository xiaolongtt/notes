# Docker

安装docker：见文档[CentOS Docker 安装 | 菜鸟教程 (runoob.com)](https://www.runoob.com/docker/centos-docker-install.html)

## Docker常用命令

> docker 基础命令
>
> 启动docker：==systemctl start docker==
>
> 关闭docker：==systemctl stop docker==
>
> 重启docker：==systemctl restart docker==
>
> docker随linux启动而自启动：==systemctl enable docker==
>
> 查看docker 运行状态------如果是在运行中 输入命令后 会看到绿色的active：==systemctl status docker==
>
> 查看docker 版本号信息：==docker -v==
>
> =======================================================================================================================================================================================================================================================================================================================================================================================
>
> docker 镜像命令
>
> 查看本地服务器中的镜像：==docker images==
>
> **拉取镜像** 不加tag(版本号) 即拉取docker仓库中 该镜像的最新版本latest 加:tag 则是拉取指定版本：
>
> ==docker pull 镜像名== 
> ==docker pull 镜像名:tag==
>
> **运行镜像** 
>
> ==docker run 镜像名==
> ==docker run 镜像名:Tag==
>
> **删除镜像 ------当前镜像没有被任何容器使用才可以删除**
>
> 删除一个
>
> ==docker rmi -f 镜像名/镜像ID==
>
> 删除多个 其镜像ID或镜像用用空格隔开即可
>
> ==docker rmi -f 镜像名/镜像ID 镜像名/镜像ID 镜像名/镜像ID==
>
> 删除全部镜像  -a 意思为显示全部, -q 意思为只显示ID
>
> ==docker rmi -f $(docker images -aq)==
>
> **保存镜像**
> 将我们的镜像 保存为tar 压缩文件 这样方便镜像转移和保存 ,然后 可以在任何一台安装了docker的服务器上 加载这个镜像
>
> ==docker save 镜像名/镜像ID -o 镜像保存在哪个位置与名字==
>
> **加载镜像**
> 任何装 docker 的地方加载镜像保存文件,使其恢复为一个镜像
>
> ==docker load -i 镜像保存文件位置==
>
> **推送镜像**：将本地镜像推送到镜像仓库中
>
> ==docker push 镜像名==
>
> =======================================================================================================================================================================================================================================================================================================================================================================================
>
> docker 容器命令
>
> **查看正在运行容器列表**：==docker ps==
>
> **查看所有容器 -----包含正在运行 和已停止的**：==docker ps -a==
>
> **运行一个容器（创建时）**：
>
> -it表示 与容器进行交互式启动 
>
> -d 表示可后台运行容器 （守护式运行）  
>
> --name 给要运行的容器 起的名字  /bin/bash  交互路径 
>
> -p表示进行端口映射，比如-p 8080:80，这样就将宿主机的8080端口与容器里运行的服务映射起来了，这样访问宿主机的8080端口就可用访问容器里的服务。
>
> ==docker run -it -d --name 要取的别名 镜像名:Tag /bin/bash==
>
> **停止容器**：==docker stop 容器名/容器ID==
>
> **重新启动容器**：==docker start 容器名/容器ID==
>
> **删除容器**：
>
> 删除一个容器
>
> ==docker rm -f 容器名/容器ID==
>
> 删除多个容器 空格隔开要删除的容器名或容器ID
>
> ==docker rm -f 容器名/容器ID 容器名/容器ID 容器名/容器ID==
>
> 删除全部容器
>
> ==docker rm -f $(docker ps -aq)==
>
> **查看容器日志**：
>
> ==docker logs -f --tail=行数 容器ID/容器名==
>
> --tail表示要查看末尾多少行 默认all，-f表示要一直查看，有新的日志就会输出
>
> **进入容器**：
> ==docker exec -it 容器名 bash==   
>
> 使用==exit==退出容器
>
> 

![](img_Cloud\Docker常用命令.png)

## Docker数据卷挂载

**数据卷（volume）**是一个虚拟目录，指向宿主机文件系统中的某个目录。

![](img_Cloud\Docker数据卷.png)

一旦完成数据卷挂载，对容器的一切操作都会作用在数据卷对应的宿主机目录了。

这样，我们操作宿主机的==/var/lib/docker/volumes/html==目录，就等于操作容器内的==/usr/share/nginx/html==

目录了。可以完成对容器里面文件的修改。

可以在启动容器的时候就将数据卷的映射关系创建好，使用以下命令：

==docker run -d -P --name nginx **-v nginx:/ect/nginx** nginx==

其中-v后面跟的就是数据卷的名称，：后面表示要映射的容器里面的页面。

**数据卷操作**：

> docker volume create：创建数据卷
>
> docker volume ls：查看所有数据卷
>
> docker volume inspect：查看数据卷详细信息，包括关联的宿主机目录位置
>
> docker volume rm：删除指定数据卷
>
> docker volume prune：删除所有未使用的数据卷

## Docker本地目录挂载

目录挂载与数据卷挂载的语法是类似的：**-v [宿主机目录]:[容器内目录]**

例如以数据库mysql为例：可以使用以下操作：

```java
docker run -d \
--name mysql \
 -v /home/docker/mysql/data:/var/lib/mysql \  //将两个目录进行映射。
 -e MYSQL_ROOT_PASSWORD=prk1229 \
 -p 3306:3306 \
 mysql
```

## Docker自定义镜像

[Docker——如何自定义镜像【将自己的项目制作成镜像】？_docker 将已经安装好的系统生成镜像-CSDN博客](https://blog.csdn.net/LYJbao/article/details/133936005?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170419663616800226593821%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170419663616800226593821&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-133936005-null-null.142^v99^pc_search_result_base9&utm_term=docker%E6%9E%84%E5%BB%BA%E8%87%AA%E5%AE%9A%E4%B9%89%E9%95%9C%E5%83%8F&spm=1018.2226.3001.4187)

![](img_Cloud\DockerFile基础操作.png)

举例该文本文件内部到底如何下：

```java
# 制定基础镜像
FROM centos:6
# 配置环境变量，JDK的安装目录，容器内时区
ENV JAVA_DIR=/usr/local
# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./xx.jar /tmp/app.jar
# 安装jdk
RUN cd $JAVA_DIR && tar -xf ./jdk8.tar.gz && mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
# 入口，java项目的启动命令
ENTRYPOINT ["java","-jar","/app.jar"]
```

​        当我们编写好Dockerfile后，使用如下命令，来构建镜像，例如：

==docker build -t myImage:1.0 .==

说明：

> ​	docker build -t 后面跟你给这个镜像起的名字，冒号后面为版本，不写的话，默认为最新版。
>
> ​	后面的一个 .  指的是dockerfile所在的目录，当前目录就为 . 

## Docker网络

​	我们在安装docker时，docker就创建一张虚拟的网卡，默认名字为docker0，并且会给这个虚拟的网卡创建一个虚拟的网桥，在我们不指定的情况下，我们创建的容器都会与默认的网卡建立连接。

​	当创建了一个容器以后。Docker会给他分配一个IP地址，这些ip地址都属于同一个网段，因此可以进行相互的通信，但是当下一次创建后，可能会导致和上一次分配的网络ip不同，导致通信出现问题，因此Docker可以进行网络ip的手动分配。可以使用==docker inspect 容器名==这个方法进行查看容器的ip地址。

**网络相关命令:**

```java
docker network create ： 创建一个网络
docker network ls ： 查看所有网络
docker network rm : 删除指定网络
docker network prune ：清楚未使用的网络
docker network connect ：使指定容器连接加入某网络
docker network disconnect：使指定容器连接离开某网络
docker network inspect ：查看网络详细信息
```

使用docker network create就能够创建一个单独的网络。就可以将容器放入这个网络中。

# RabbitMQ

[RabbitMQ快速入门（详细）-CSDN博客](https://blog.csdn.net/kavito/article/details/91403659?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170506163016800215056460%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170506163016800215056460&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-91403659-null-null.142^v99^pc_search_result_base9&utm_term=RabbitMQ&spm=1018.2226.3001.4187)

![](img_Cloud\RabbitMQ整体概念.png)

生产者发送消息流程：

1、生产者和Broker建立TCP连接。

2、生产者和Broker建立通道。

3、生产者通过通道消息发送给Broker，由Exchange将消息进行转发。

4、Exchange将消息转发到指定的Queue（队列）

消费者接收消息流程：

1、消费者和Broker建立TCP连接

2、消费者和Broker建立通道

3、消费者监听指定的Queue（队列）

4、当有消息到达Queue时Broker默认将消息推送给消费者。

5、消费者接收到消息。

6、ack回复

## 六种消息模式

### 一、基本消息模型

![](img_Cloud\MQ工作方式一.png)

在上图的模型中，有以下概念：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分。可以缓存消息；生产者向其中投递消息，消费者从其中取出消息。 

操作步骤：

第一步：引入依赖

```java
<!--AMQP依赖，包含RabbitMQ-->
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

第二步：分别在消费者和生产者的yml文件中建立与RabbitMq的连接

```java
spring:
  rabbitmq:
    host: 192.168.26.128		//定义RabbitMQ的地址
    port: 5672					//RabbitMQ的服务端口
    virtual-host: /				//RabiitMQ属于那一层
    username: itheima			//用户名和密码
    password: 123321
```

第三步：书写生产者代码，主要先要注入RabbitTemplate这个类。然后通过这个类里的convertAndSend方法进行消息发送，事先需要在RabbitMQ的客户端中先创建一个队列，然后进行消息发送。

```java
public class PublisherTest {
    @Autowired
    private RabbitTemplate rabbitTemplate;
    @Test
    public void sendMessage(){

        String queueName="simple.queue";	//在客户端中创建的队列名
        String Msg="hello mq";				//要发送的消息
        rabbitTemplate.convertAndSend(queueName,Msg);
    }    
}
```

第四步：书写消费者代码，需要创建一个类，这个类要交给Spring管理(加上@Component注解)，然后里面的方法加上==@RabbitListener(queues = "simple.queue")==，这个注解。其中queues表示要消费的队列名称，可以有多个，然后方法的参数就是接收到的消息内容。

```java
@Component
public class ConsumerTest {

    @RabbitListener(queues = "simple.queue")
    public void getMessage(String msg){
        log.info("收到的消息为：{}",msg);
    }
}
```

### 二、work消息模型

![](img_Cloud\MQ工作方式二.png)

work queues与入门程序相比，多了一个消费端，两个消费端共同消费同一个队列中的消息，但是一个消息只能被一个消费者获取。

这个消息模型在Web应用程序中特别有用，可以处理短的HTTP请求窗口中无法处理复杂的任务。

接下来我们来模拟这个流程：

- P：生产者：任务的发布者
- C1：消费者1：领取任务并且完成任务，假设完成速度较慢（模拟耗时）
- C2：消费者2：领取任务并且完成任务，假设完成速度较快

比如现在让生产者一秒钟以内产生50条消息：

> ​	默认情况下，RabbitMQ的会将消息依次轮询投递给绑定在队列上的每一个消费者(即每个消费者一人25条消息)。 但这并没有考虑到消费者是否已经处理完消息,可能出现消息堆积。当一个消费者的处理消息的能力较差时，这样另外一个消费者已经处理完所有消息以后还有消息堆积。
>
> ​	因此我们需要修改消费者端application.yml,设置preFetch值为1, 这样就能使一次性给一个消费者一条消息，当他将这条消息处理完以后才会给他分配下一条消息，这样就能实现能者多劳，处理消息快的消费者能处理更多的消息。

在消费端的配置文件中进行修改：

```java
spring:
  rabbitmq:
    host: 192.168.26.128
    port: 5672
    virtual-host: /
    username: itheima
    password: 123321
    listener:
      simple:
        prefetch: 1				//修改成为1
```

例子如下：
生产者代码：1秒钟以内发送50条消息到消息队列

```java
@Test
    public void sendMessage2() throws InterruptedException {
        String queueName="simple.queue";
        for(int i=0;i<50;i++){
            String Msg="hello mq__"+i;
            rabbitTemplate.convertAndSend(queueName,Msg);
            System.out.println(i);
            Thread.sleep(20);
        }
    }
```

消费者代码：用两个消费者来绑定同一个队列。进行消息接收

```java
@RabbitListener(queues = "simple.queue")
public void getMessage(String msg) throws InterruptedException {
    log.info("消费者1收到的消息为：{}",msg);
    Thread.sleep(200);
}

@RabbitListener(queues = "simple.queue")
public void getMessage2(String msg) throws InterruptedException {
    log.info("消费者2收到的消息为.......：{}",msg);
}
```

结果如下：两个消费者一人消费了25条消息，即使在消费者1有200毫秒睡眠的情况下，所以可以模拟出消费者1的处理能力较弱。所以这样并不满足能者多劳。

> 消费者1收到的消息为：hello mq__0
> 消费者2收到的消息为.......：hello mq__1
> 消费者2收到的消息为.......：hello mq__3
> 消费者2收到的消息为.......：hello mq__5
> 消费者1收到的消息为：hello mq__2
> 消费者2收到的消息为.......：hello mq__7
> 消费者2收到的消息为.......：hello mq__9
> 消费者2收到的消息为.......：hello mq__11
> 消费者2收到的消息为.......：hello mq__13
> 消费者1收到的消息为：hello mq__4

当修改配置文件以后，结果如下：

> 消费者1收到的消息为：hello mq__0
> 消费者2收到的消息为.......：hello mq__1
> 消费者2收到的消息为.......：hello mq__2
> 消费者2收到的消息为.......：hello mq__3
> 消费者2收到的消息为.......：hello mq__4
> 消费者2收到的消息为.......：hello mq__5
> 消费者2收到的消息为.......：hello mq__6
> 消费者2收到的消息为.......：hello mq__7
> 消费者2收到的消息为.......：hello mq__8
> 消费者1收到的消息为：hello mq__9
> 消费者2收到的消息为.......：hello mq__10

此时满足了消费者2能者多劳，能够处理更多的消息。

### 三、Fanout交换机

![](img_Cloud\MQ工作方式三.png)

​	和前面两种模式不同：这种工作方法使用了一个Fanout交换机，用来进行广播消息，只要是与交换机进行绑定的队列，当生产者发送了消息，就会广播给每一个队列，从而实现广播。

操作步骤：

第一步：先在RabbitMQ的客户端创建一个Fanout交换机，并且创建两个队列与交换机进行绑定。

第二步：创建生产者代码，此时发送的对象不是一个队列，而是一个交换机，消息再通过交换机进行转发。

```java
public void sendMessage_Fanout(){
    String exchange_name="Fanout.exchange";
    String Msg="hello everyone";
    rabbitTemplate.convertAndSend(exchange_name,null,Msg);
}
```

第三步：创建消费者代码，还是接收两个与Fanout交换机绑定过的队列。

```java
@RabbitListener(queues = "Fanout.queue1")
public void getMessage_fanout1(String msg) throws InterruptedException {
    log.info("消费者1收到的消息为：{}",msg);
}

@RabbitListener(queues = "Fanout.queue2")
public void getMessage_fanout2(String msg) throws InterruptedException {
    log.info("消费者2收到的消息为：{}",msg);
}
```

结果是，两个队列都收到了来自交换机广播的消息。

### 四、Direct交换机

![](img_Cloud\MQ工作方式四.png)

P：生产者，向Exchange发送消息，发送消息时，会指定一个==routing key==。

X：Exchange（交换机），接收生产者的消息，然后把消息递交给与==routing key==完全匹配的队列

C1：消费者，其所在队列指定了需要routing key 为 error 的消息

C2：消费者，其所在队列指定了需要routing key 为 info、error、warning 的消息

操作步骤：

第一步：先再RabbitMQ的控制台创建一个Direct交换机，再创建两个队列，然后让两个队列都与Direct交换机进行绑定，并且分别绑定两个routing key。

第二步：创建生产者代码，发送消息给一个交换机，并且指定了一个==routing key==为error，那么只有符合要求的队列才会收到这个消息。

```java
@Test
public void sendMessage_Direct(){
    String exchange_name="Direct.exchange";
    String Msg="hello ";
    rabbitTemplate.convertAndSend(exchange_name,"error",Msg);
}
```

第三步：创建消费者代码，routing key的绑定在MQ的网页客户端中已经完成。

```java
@RabbitListener(queues = "Direct.queue1")
public void getMessage_Direct1(String msg) throws InterruptedException {
    log.info("消费者1收到的消息为：{}",msg);
}

@RabbitListener(queues = "Direct.queue2")
public void getMessage_Direct2(String msg) throws InterruptedException {
    log.info("消费者2收到的消息为：{}",msg);
}
```

结果为，只有routing key为error的队列才收到了消息。就实现了不同的队列处理不同的消息的功能。

### 五、Topic交换机

![](img_Cloud\MQ工作方式五.png)

每个消费者监听自己的队列，并且设置带统配符的routingkey,生产者将消息发给broker，由交换机根据routingkey来转发消息到指定的队列。

Routingkey一般都是有一个或者多个单词组成，多个单词之间以“.”分割，例如：inform.sms

通配符规则：

> ’#‘：匹配一个或多个词
>
> ’*‘：匹配不多不少恰好1个词

举例：

> audit.#：能够匹配audit.irs.corporate 或者 audit.irs
>
> audit.*：只能匹配audit.irs

操作步骤：

第一步：在RabbitMq的客户端，创建一个Topic交换机，并且创建两个队列，同时绑定上带通配符的==routing key==。

第二步：书写生产者代码，这样发送的消息只有==routing key==为china.#或者#.news的队列才能收到。

```java
public void sendMessage_Topic(){
    String exchange_name="Topic.exchange";
    String Msg="hello ";
    rabbitTemplate.convertAndSend(exchange_name,"china.news",Msg);
}
```

第三步：消费者代码

```java
@RabbitListener(queues = "Topic.queue1")
public void getMessage_Topic1(String msg) throws InterruptedException {
    log.info("消费者1收到的消息为：{}",msg);
}

@RabbitListener(queues = "Topic.queue2")
public void getMessage_Topic2(String msg) throws InterruptedException {
    log.info("消费者2收到的消息为：{}",msg);
}
```

## 在java中用代码创建队列、交换机以及绑定关系

一般会创建一个配置类，用来专门创建队列，交换机和绑定关系。

```java
@Configuration
public class FanoutConfiguration {
    /**
     * 创建交换机
     * @return
     */
    @Bean
    public FanoutExchange fanoutExchange(){
        //ExchangeBuilder.fanoutExchange("fanout_exchange").build();
        return new FanoutExchange("fanout.exchange");
    }

    /**
     * 创建队列
     * @return
     */
    @Bean
    public Queue queue_fanout1(){
        //QueueBuilder.durable("fanout.queue1").build();
        return new Queue("fanout.queue1");
    }

    /**
     * 绑定交换机和队列
     * @param fanoutExchange
     * @param queue_fanout1
     * @return
     */
    @Bean
    public Binding fanoutBinding_queue1(FanoutExchange fanoutExchange,Queue queue_fanout1){
        return BindingBuilder.bind(queue_fanout1).to(fanoutExchange);
    }

    //绑定方法二
    @Bean
    public Binding fanoutBinding_queue2(){
        return BindingBuilder.bind(queue_fanout1()).to(fanoutExchange());
    }

}
```

使用注解创建：更加方便

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "queue1",declare = "true"),
        exchange = @Exchange(name = "exchange1",type = ExchangeTypes.DIRECT),
        key={"red","blue"}
))
public void getMessage_test(String msg) throws InterruptedException {
    log.info("消费者收到的消息为.......：{}",msg);
}
```

## 使用RabbitMq发送对象

创建一个队列，发送map对象

```java
public void sendMessage_Object(){
    String name="object.queue";
    Map<String,Object> msg=new HashMap<>();
    msg.put("name","tom");
    msg.put("age",18);
    rabbitTemplate.convertAndSend(name,msg);
}
```

消费者代码：

```java
@RabbitListener(queues = "object.queue")
public void getMessage_object(Map<String,Object> msg) throws InterruptedException {
    log.info("消费者收到的消息为.......：{}",msg);
}
```

当发送对象时，会使用默认的序列化器将map对象转化。但是使用默认的转化器转化出的数据占用的字节数太长，而且可能会存在安全问题。所以可以自定义一个序列化器，转化为json格式，节约空间和安全。代码如下：

先引入maven依赖：

```java
<!--Jackson-->
<dependency>
      <groupId>com.fasterxml.jackson.dataformat</groupId>
      <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

在创建消息转换器：在消费者和生产者两边都要创建，防止两边使用的消息转换器不同，导致解析消息出错。

```java
@Bean
public MessageConverter jacksonMessageConverter(){
    return new Jackson2JsonMessageConverter();
}
```

## 生产者的可靠性

### 生产者重连

​	当因为网络波动或者其它原因导致生产者连接RabbitMq的服务器出现问题，这时候就需要进行重连，防止出现消息发送不出的问题。这种问题就需要使用生产者重连技术。实现这个功能的方法很简单，只需要在配置文件中修改相关配置即可：

```java
spring:
  rabbitmq:
#   设置RabbitMq的连接超时时间
    connection-timeout: 1s
    template:
      retry:
#       开启超时重连
        enabled: true
#       超时重连失败后下次等待时间的倍数
        multiplier: 1
#       超时后的初始等待时间
        initial-interval: 1000ms
#       超时重连最大次数
        max-attempts: 3
```

### 生产者确认

​	RabbitMQ提供了publisher confirm机制来避免消息发送到MQ过程中丢失。这种机制必须给每个消息指定一个唯一ID。消息发送到MQ以后，会返回一个==ACK==给发送者，表示消息是否处理成功。

返回结果有两种方式：

publisher-confirm，发送者确认 

> 消息成功投递到交换机，返回ack ===> 消息分为临时消息和持久性消息，持久性消息需要存入磁盘才返回ack
>
> 消息未投递到交换机，返回nack

publisher-return，发送者回执

> 消息投递到交换机了，但是没有路由到队列。返回ACK，及路由失败原因。

实现步骤：
1、先在配置文件中进行配置

```java
spring:
  rabbitmq:
    publisher-confirm-type: correlated
    publisher-returns: true
```

说明：

- publish-confirm-type：开启publisher-confirm，这里支持两种类型：
  - simple：同步等待confirm结果，直到超时
  - correlated：异步回调，定义ConfirmCallback，MQ返回结果时会回调这个ConfirmCallback
- publish-returns：开启publish-return功能，同样是基于callback机制，不过是定义ReturnCallback

2、实现Return回调

每个RabbitTemplate只能配置一个ReturnCallback，因此需要在项目加载时配置：

修改publisher服务，添加一个配置类：

```java
@Slf4j
@Configuration
public class MqConfirmConfig implements ApplicationContextAware {
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        //从容器中取出想要的Bean
        RabbitTemplate rabbitTemplate = applicationContext.getBean(RabbitTemplate.class);
        //给这个Bean配置回调
        //这种方法以及被启用，使用下面的方法
        /*rabbitTemplate.setReturnCallback((message, replyCode, replyText, exchange, routingKey) -> {
            // 投递失败，记录日志
            log.info("消息发送失败，应答码{}，原因{}，交换机{}，路由键{},消息{}",
                    replyCode, replyText, exchange, routingKey, message.toString());
            // 如果有业务需要，可以重发消息
        });*/

        //最新的方法
        rabbitTemplate.setReturnsCallback(new RabbitTemplate.ReturnsCallback() {
            @Override
            public void returnedMessage(ReturnedMessage returnedMessage) {
                log.info("消息发送失败，应答码{}，原因{}，交换机{}，路由键{},消息{}",
 				returnedMessage.getReplyCode(),returnedMessage.getReplyText(),
  				returnedMessage.getExchange(),returnedMessage.getRoutingKey(),
                returnedMessage.getMessage().toString());
            }
        });
    }
}
```

3、定义ConfirmCallback

ConfirmCallback可以在发送消息时指定，因为每个业务处理confirm成功或失败的逻辑不一定相同。所以在每个发送消息的方法中进行定义和配置。

```java
@Test
public void sendMessageTest() throws InterruptedException {
    String exchangeName="simple.queue";
    String Msg="hello mq";
    // 全局唯一的消息ID，需要封装到CorrelationData中
    CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
    // 添加callback
    correlationData.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
        @Override
        public void onFailure(Throwable ex) {
            log.info("消息回调失败",ex);
        }
        @Override
        public void onSuccess(CorrelationData.Confirm result) {
            log.info("收到消息回调");
            if(result.isAck()){
                log.debug("消息发送成功，收到Ack");
            }else {
                log.error("消息发送失败，收到NAck,原因为：{}",result.getReason());
            }
        }
    });
    rabbitTemplate.convertAndSend(exchangeName,"red",Msg,correlationData);
    //休眠2秒钟，防止收不到回调信息
    Thread.sleep(2000);
}
```

这样，在生产者发送消息以后，就会在控制台上收到消息，从而知道消息的情况如何，是否发送成功等等。

## 消息持久化

​	当保证了生产者的可靠性，确保消息能够发送到Mq，但是有可能消息在Mq中的时候造成了消息的丢失。所以要使用消息持久化。

 	消息持久化分为：交换机持久化，队列持久化，消息持久化

**交换机持久化**
RabbitMQ中交换机默认是非持久化的，mq重启后就丢失。

SpringAMQP中可以通过代码指定交换机持久化：

```java
@Bean
public DirectExchange simpleExchange(){
    // 三个参数：交换机名称、是否持久化、当没有queue与其绑定时是否自动删除
    return new DirectExchange("simple.direct", true, false);
}
```

事实上，默认情况下，由SpringAMQP声明的交换机都是持久化的。

**队列持久化**
RabbitMQ中队列默认是非持久化的，mq重启后就丢失。

SpringAMQP中可以通过代码指定交换机持久化：

```java
@Bean
public Queue simpleQueue(){
    // 使用QueueBuilder构建队列，durable就是持久化的
    return QueueBuilder.durable("simple.queue").build();
}
```

事实上，默认情况下，由SpringAMQP声明的队列都是持久化的。

**消息持久化**

利用SpringAMQP发送消息时，可以设置消息的属性（MessageProperties），指定delivery-mode：

- 1：非持久化
- 2：持久化

用java代码指定：

```java
Message message= MessageBuilder
                .withBody("hello mq".getBytes(StandardCharsets.UTF_8))
                .setDeliveryMode(MessageDeliveryMode.PERSISTENT)//消息持久化
                .build();
```

默认情况下，SpringAMQP发出的任何消息都是持久化的，不用特意指定。

## Lazy Queue

​	从3.6.0开始引入了**惰性队列（Lazy Queue）**的概念。惰性队列会尽可能的将消息存入磁盘中，而在消费者消费到相应的消息时才会被加载到内存中，它的一个重要设计目标是能够支持更长的队列，即支持更多的消息存储。当消费者由于各种各样的原因（比如消费者下线、跌机、或者由于维护而关闭等）致使长时间不能消费消息而造成堆积时，惰性队列就很必要了。惰性队列比**3.12版本以后所以的队列都默认为惰性队列。**
	使用惰性队列的方法很简单，可以在RabbitMq的控制台进行选择，在java代码中的操作如下：

```java
@Bean
public Queue queue_fanout1(){
    return QueueBuilder
        .durable("lazy.queue1")
        .lazy()			//表示这个队列为惰性队列
        .build();
}
```

使用注解的操作如下：

```java
@RabbitListener(bindings = @QueueBinding(
       value = @Queue(name = "queue1",declare = "true",
                      arguments = 
                      @Argument(name="x-queue-mode",value = "lazy")),//惰性队列
       exchange = @Exchange(name = "exchange1",type = ExchangeTypes.DIRECT),
       key={"red","blue"}
))
public void getMessage_test(String msg) throws InterruptedException {
    log.info("消费者收到的消息为.......：{}",msg);
}
```

## 消费者的可靠性

### 消费者确认机制

​	为了确认消费者是否成功处理消息，RabbitMQ提供了消费者确认机制（**Consumer Acknowledgement**）。即：当消费者处理消息结束后，应该向RabbitMQ发送一个回执，告知RabbitMQ自己消息处理状态。回执有三种可选值：

- ack：成功处理消息，RabbitMQ从队列中删除该消息
- nack：消息处理失败，RabbitMQ需要再次投递消息
- reject：消息处理失败并拒绝该消息，RabbitMQ从队列中删除该消息

一般reject方式用的较少，除非是消息格式有问题，那就是开发问题了。因此大多数情况下我们需要将消息处理的代码通过`try catch`机制捕获，消息处理成功时返回ack，处理失败时返回nack.



​	由于消息回执的处理代码比较统一，因此SpringAMQP帮我们实现了消息确认。并允许我们通过配置文件设置ACK处理方式，有三种模式：

- **none**：不处理。即消息投递给消费者后立刻ack，消息会立刻从MQ删除。非常不安全，不建议使用
- **manual**：手动模式。需要自己在业务代码中调用api，发送`ack`或`reject`，存在业务入侵，但更灵活
- **auto**：自动模式。SpringAMQP利用AOP对我们的消息处理逻辑做了环绕增强，当业务正常执行时则自动返回`ack`.  当业务出现异常时，根据异常判断返回不同结果：
  - 如果是**业务异常**，会自动返回`nack`；
  - 如果是**消息处理或校验异常**，自动返回`reject`;

使用时只需要在消费者端的配置文件中进行修改配置即可。

```java
spring:
  rabbitmq:
    listener:
      simple:
        acknowledge-mode: none # 不做处理
```

当使用==none==时，如果出现异常，消息也会被RabbitMq删除掉，从而导致消息丢失。

当我们把配置改为`auto`时，消息处理失败后，会回到RabbitMQ，并重新投递到消费者。

### 消费者的失败重试机制

​	当消费者出现异常后，消息会不断requeue（重入队）到队列，再重新发送给消费者。如果消费者再次执行依然出错，消息会再次requeue到队列，再次投递，直到消息处理成功为止。

​	极端情况就是消费者一直无法执行成功，那么消息requeue就会无限循环，导致mq的消息处理飙升，带来不必要的压力。

​	为了应对上述情况Spring又提供了==消费者失败重试机制==：在消费者出现异常时利用本地重试，而不是无限制的requeue到mq队列。

修改consumer服务的application.yml文件，添加内容：

```java
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true # 开启消费者失败重试
          initial-interval: 1000ms # 初识的失败等待时长为1秒
          multiplier: 1 # 失败的等待时长倍数，下次等待时长 = multiplier * last-interval
          max-attempts: 3 # 最大重试次数
          stateless: true # true无状态；false有状态。如果业务中包含事务，这里改为false
```

重启consumer服务，重复之前的测试。可以发现：

- 消费者在失败后消息没有重新回到MQ无限重新投递，而是在本地重试了3次
- 本地重试3次以后，抛出了`AmqpRejectAndDontRequeueException`异常。查看RabbitMQ控制台，发现消息被删除了，说明最后SpringAMQP返回的是`reject`

结论：

- 开启本地重试时，消息处理过程中抛出异常，不会requeue到队列，而是在消费者本地重试
- 重试达到最大次数后，Spring会返回reject，消息会被丢弃

### 失败处理策略

在之前的测试中，本地测试达到最大重试次数后，消息会被丢弃。这在某些对于消息可靠性要求较高的业务场景下，显然不太合适了。

因此Spring允许我们自定义重试次数耗尽后的消息处理策略，这个策略是由`MessageRecovery`接口来定义的，它有3个不同实现：

- `RejectAndDontRequeueRecoverer`：重试耗尽后，直接`reject`，丢弃消息。默认就是这种方式 
- `ImmediateRequeueMessageRecoverer`：重试耗尽后，返回`nack`，消息重新入队 
- `RepublishMessageRecoverer`：重试耗尽后，将失败消息投递到指定的交换机 

比较优雅的一种处理方案是`RepublishMessageRecoverer`，失败后将消息投递到一个指定的，专门存放异常消息的队列，后续由人工集中处理。

方式三的处理步骤：下面代码书写在同一个类中。也就是在一个配置类中。

第一步：定义接收失败消息的队列和交换机，及其绑定关系。

第二步：定义RepublishMessageRecoverer,关联队列和交换机。

完整代码如下：第二个在类上的注解表示只有用户在开启了**失败重试机制**以后这个配置类才会生效。

```java
@Configuration
@ConditionalOnProperty(name = "spring.rabbitmq.listener.simple.retry.enabled", havingValue = "true")
public class ErrorMessageConfig {
    @Bean
    public DirectExchange errorMessageExchange(){
        return new DirectExchange("error.direct");
    }
    @Bean
    public Queue errorQueue(){
        return new Queue("error.queue", true);
    }
    @Bean
    public Binding errorBinding(Queue errorQueue, DirectExchange errorMessageExchange){
        return BindingBuilder.bind(errorQueue).to(errorMessageExchange).with("error");
    }

    @Bean
    public MessageRecoverer republishMessageRecoverer(RabbitTemplate rabbitTemplate){
        return new RepublishMessageRecoverer(rabbitTemplate, "error.direct", "error");
    }
}
```

### 业务幂等性

**幂等**是一个数学概念，用函数表达式来描述是这样的：`f(x) = f(f(x))`，例如求绝对值函数。

在程序开发中，则是指同一个业务，执行一次或多次对业务状态的影响是一致的。例如：

- 根据id删除数据
- 查询数据
- 新增数据

但数据的更新往往不是幂等的，如果重复执行可能造成不一样的后果。比如：

- 取消订单，恢复库存的业务。如果多次恢复就会出现库存重复增加的情况
- 退款业务。重复退款对商家而言会有经济损失。

所以，我们要尽可能避免业务被重复执行。

然而在实际业务场景中，由于意外经常会出现业务被重复执行的情况，例如：

- 页面卡顿时频繁刷新导致表单重复提交
- 服务间调用的重试
- MQ消息的重复投递

我们在用户支付成功后会发送MQ消息到交易服务，修改订单状态为已支付，就可能出现消息重复投递的情况。如果消费者不做判断，很有可能导致消息被消费多次，出现业务故障。

举例：

1. 假如用户刚刚支付完成，并且投递消息到交易服务，交易服务更改订单为**已支付**状态。
2. 由于某种原因，例如网络故障导致生产者没有得到确认，隔了一段时间后**重新投递**给交易服务。
3. 但是，在新投递的消息被消费之前，用户选择了退款，将订单状态改为了**已退款**状态。
4. 退款完成后，新投递的消息才被消费，那么订单状态会被再次改为**已支付**。业务异常。

因此，我们必须想办法保证消息处理的幂等性。这里给出两种方案：

- 唯一消息ID
- 业务状态判断

#### 唯一消息ID

这个思路非常简单：

1. 每一条消息都生成一个唯一的id，与消息一起投递给消费者。
2. 消费者接收到消息后处理自己的业务，业务处理成功后将消息ID保存到数据库
3. 如果下次又收到相同消息，去数据库查询判断是否存在，存在则为重复消息放弃处理。

我们该如何给消息添加唯一ID呢？

其实很简单，SpringAMQP的MessageConverter自带了MessageID的功能，我们只要开启这个功能即可。

以Jackson的消息转换器为例：

```java
@Bean
public MessageConverter messageConverter(){
    // 1.定义消息转换器
    Jackson2JsonMessageConverter jjmc = new Jackson2JsonMessageConverter();
    // 2.配置自动创建消息id，用于识别不同消息，也可以在业务中基于ID判断是否是重复消息
    jjmc.setCreateMessageIds(true);
    return jjmc;
}
```

#### 业务判断

业务判断就是基于业务本身的逻辑或状态来判断是否是重复的请求或消息，不同的业务场景判断的思路也不一样。

### 兜底方案

虽然我们利用各种机制尽可能增加了消息的可靠性，但也不好说能保证消息100%的可靠。万一真的MQ通知失败该怎么办呢？

有没有其它兜底方案，能够确保订单的支付状态一致呢？

其实思想很简单：既然MQ通知不一定发送到交易服务，那么交易服务就必须自己**主动去查询**支付状态。这样即便支付服务的MQ通知失败，我们依然能通过主动查询来保证订单状态的一致。

这个时间是无法确定的，因此，通常我们采取的措施就是利用**定时任务**定期查询，例如每隔20秒就查询一次，并判断支付状态。如果发现订单已经支付，则立刻更新订单状态为已支付即可。

综上，支付服务与交易服务之间的订单状态一致性是如何保证的？

- 首先，支付服务会正在用户支付成功以后利用MQ消息通知交易服务，完成订单状态同步。
- 其次，为了保证MQ消息的可靠性，我们采用了生产者确认机制、消费者确认、消费者失败重试等策略，确保消息投递的可靠性
- 最后，我们还在交易服务设置了**定时任务**，定期查询订单支付状态。这样即便MQ通知失败，还可以利用定时任务作为兜底方案，确保订单支付状态的最终一致性。

## 延迟消息

在电商的支付业务中，对于一些库存有限的商品，为了更好的用户体验，通常都会在用户下单时立刻扣减商品库存。例如电影院购票、高铁购票，下单后就会锁定座位资源，其他人无法重复购买。

但是这样就存在一个问题，假如用户下单后一直不付款，就会一直占有库存资源，导致其他客户无法正常交易，最终导致商户利益受损！

因此，电商中通常的做法就是：**对于超过一定时间未支付的订单，应该立刻取消订单并释放占用的库存**。

例如，订单支付超时时间为30分钟，则我们应该在用户下单后的第30分钟检查订单支付状态，如果发现未支付，应该立刻取消订单，释放库存。

但问题来了：如何才能准确的实现在下单后第30分钟去检查支付状态呢？

像这种在一段时间以后才执行的任务，我们称之为**延迟任务**，而要实现延迟任务，最简单的方案就是利用MQ的延迟消息了。

在RabbitMQ中实现延迟消息也有两种方案：

- 死信交换机+TTL
- 延迟消息插件

### 死信交换机

当一个队列中的消息满足下列情况之一时，可以成为死信（dead letter）：

- 消费者使用`basic.reject`或 `basic.nack`声明消费失败，并且消息的`requeue`参数设置为false
- 消息是一个过期消息，超时无人消费
- 要投递的队列消息满了，无法投递

如果一个队列中的消息已经成为死信，并且这个队列通过**dead-letter-exchange**属性指定了一个交换机，那么队列中的死信就会投递到这个交换机中，而这个交换机就称为**死信交换机**（Dead Letter Exchange）。而此时加入有队列与死信交换机绑定，则最终死信就会被投递到这个队列中。

死信交换机有什么作用呢？

1. 收集那些因处理失败而被拒绝的消息
2. 收集那些因队列满了而被拒绝的消息
3. 收集因TTL（有效期）到期的消息

前面两种作用场景可以看做是把死信交换机当做一种消息处理的最终兜底方案，与消费者重试时讲的`RepublishMessageRecoverer`作用类似。

总结：

- RabbitMQ的消息过期是基于追溯方式来实现的，也就是说当一个消息的TTL到期以后不一定会被移除或投递到死信交换机，而是在消息恰好处于队首时才会被处理。
- 当队列中消息堆积很多的时候，过期消息可能不会被按时处理，因此你设置的TTL时间不一定准确。

### DelayExchange插件

基于死信队列虽然可以实现延迟消息，但是太麻烦了。因此RabbitMQ社区提供了一个延迟消息插件来实现相同的效果。

在[github.com](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange)下载

[Scheduling Messages with RabbitMQ | RabbitMQ - Blog](https://blog.rabbitmq.com/posts/2015/04/scheduling-messages-with-rabbitmq/)这个是官方文档



# Nginx

​	Nginx 是**高性能的 HTTP** 和**反向代理**的web服务器，处理**高并发**能力是十分强大的，能经受高负载的考验,有报告表明能支持高达 50,000 个并发连接数。

Nginx的主要功能：

1.正向代理
	如果把局域网外的 Internet 想象成一个巨大的资源库，则局域网中的客户端要访 问 Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理。

2.反向代理
	反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问。我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器 IP 地址。

3.负载均衡
	增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡

​	客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服务器处理完毕后，再将结果返回给客户端。

4.动静分离

​	为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速 度。降低原来单个服务器的压力。



# SpringCloud

## 微服务介绍

微服务的架构特征：

- 单一职责：微服务拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责
- 自治：团队独立、技术独立、数据独立，独立部署和交付
- 面向服务：服务提供统一标准的接口，与语言和技术无关
- 隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题
- 每一个服务有一个专用的数据库，这个服务只能使用这个数据库，要想访问其它服务的数据库，就要去请求其它服务的接口，可用使用RestTemplate进行发送请求。

SpringCloud是目前国内使用最广泛的微服务框架。官网地址：https://spring.io/projects/spring-cloud。

SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的开箱即用体验。

其中常见的组件包括：

![](img_Cloud\springcloud组件.png)

另外，SpringCloud底层是依赖于SpringBoot的，并且有版本的兼容关系，如下：

![](img_Cloud\Springcloud与Springboot的对应版本.png)

**总结**

- 单体架构：简单方便，高度耦合，扩展性差，适合小型项目。例如：学生管理系统

- 分布式架构：松耦合，扩展性好，但架构复杂，难度大。适合大型互联网项目，例如：京东、淘宝

- 微服务：一种良好的分布式架构方案

  ①优点：拆分粒度更小、服务更独立、耦合度更低

  ②缺点：架构非常复杂，运维、监控、部署难度提高

- SpringCloud是微服务架构的一站式解决方案，集成了各种优秀微服务功能组件

## Eureka注册中心

​	在微服务架构中往往会有一个注册中心，每个微服务都会向**注册中心**去注册自己的地址及端口信息，注册中心维护着服务名称与服务实例的对应关系。每个微服务都会定时从注册中心获取服务列表，同时汇报自己的运行情况，这样当有的服务需要调用其他服务时，就可以从自己获取到的服务列表中获取实例地址进行调用，注册中心还能实现对每个服务器的负载均衡。

Eureka注册中心的结构如下：

![](img_Cloud\Eureka注册中心.png)

### 搭建注册中心的步骤：

第一步：创建eureka-server服务，在项目中创建一个子模块(eureka-server)。

第二步：导入eureka的maven依赖：

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

第三步：编写启动类，并且加上==@EnableEurekaServer==注解，开启eureka的注册中心功能：

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }
}
```

第四步：编写配置文件：编写一个application.yml文件.

```java
server:
  port: 10086    //访问eureka的端口
spring:
  application:
    name: eureka-server  //服务的名称，eureka-server服务本身也是一个微服务，也会被管理
eureka:
  client:
    service-url: 
      defaultZone: http://127.0.0.1:10086/eureka
```

第五步：启动微服务，在浏览器访问：http://127.0.0.1:10086，就能访问Eureka的客户端

### 服务注册的步骤

比如将user-service注册到eureka-server中去。

第一步：在user-service的pom文件中，引入下面的==eureka-client==依赖：

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

第二步：在user-service中，修改application.yml文件，添加服务名称、eureka地址：

```java
spring:
  application:
    name: userservice  //这个服务的名称
//注册到的地址，与上面一致
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

### 使用注册中心的服务的步骤

比如下面，我们将order-service的逻辑修改：向eureka-server拉取user-service的信息，实现服务发现。

1）引入依赖

服务发现、服务注册统一都封装在eureka-client依赖，因此这一步与服务注册时一致。

在order-service的pom文件中，引入下面的eureka-client依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2）配置文件

服务发现也需要知道eureka地址，因此第二步与服务注册一致，都是配置eureka信息：

在order-service中，修改application.yml文件，添加服务名称、eureka地址：

```yaml
spring:
  application:
    name: orderservice
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

3）服务拉取和负载均衡

最后，我们要去eureka-server中拉取user-service服务的实例列表，并且实现负载均衡。

在order-service的启动类中，给==**RestTemplate这个Bean添加一个@LoadBalanced注解**==，这样就能够实现负载均衡。

同时在进行请求其它服务的接口时URL地址就不用写端口号，只用写要访问的那个服务在eureka注册中心中注册的名称，比如：
可以将`：http://localhost:8081/user.....改为http://userservice/user.....`

spring会自动帮助我们从eureka-server端，根据userservice这个服务名称，获取实例列表，而后完成负载均衡。

## Ribbon负载均衡

### 负载均衡原理

SpringCloudRibbon的底层采用了一个拦截器，拦截了RestTemplate发出的请求，对地址做了修改。用一幅图来总结一下：

![](img_Cloud\负载均衡原理.png)

基本流程如下：

- 拦截我们的RestTemplate请求http://userservice/user/1。
- RibbonLoadBalancerClient会从请求url中获取服务名称，也就是user-service。
- DynamicServerListLoadBalancer根据user-service到eureka拉取服务列表。
- eureka返回列表，localhost:8081、localhost:8082。
- IRule利用内置负载均衡规则，从列表中选择一个，例如localhost:8081。
- RibbonLoadBalancerClient修改请求地址，用localhost:8081替代userservice，得到http://localhost:8081/user/1，发起真实请求。

### 负载均衡策略

负载均衡的规则都定义在IRule接口中，而IRule有很多不同的实现类：

![](img_Cloud\负载均衡类.png)

不同规则的含义如下：

| **内置负载均衡规则类**    | **规则描述**                                                 |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：   （1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。  （2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上限，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| **ZoneAvoidanceRule**     | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。而后再对Zone内的多个服务做轮询。 |
| BestAvailableRule         | 忽略那些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| RetryRule                 | 重试机制的选择逻辑                                           |

默认的实现就是ZoneAvoidanceRule，是一种轮询方案

### 自定义负载均衡策略

通过定义IRule实现可以修改负载均衡规则，有两种方式：

1. 代码方式：在order-service中的OrderApplication类中，定义一个新的IRule：

   这种方法一配置所有的服务都会使用这种方法。

```java
@Bean
public IRule randomRule(){
    return new RandomRule();
}
```

1. 配置文件方式：在order-service的application.yml文件中，添加新的配置也可以修改规则：

   这种方法配置的只有某一个服务生效。

```yaml
userservice: # 给某个微服务配置负载均衡规则，这里是userservice服务
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 负载均衡规则 
```

> **注意**，一般用默认的负载均衡规则，不做修改。

### 饥饿加载

Ribbon默认是采用懒加载，即第一次访问时才会去创建LoadBalanceClient，请求时间会很长。

而饥饿加载则会在项目启动时创建，降低第一次访问的耗时，通过下面配置开启饥饿加载：

```yaml
ribbon:
  eager-load:
    enabled: true
    clients: userservice
```

## Nacos注册中心

[Nacos](https://nacos.io/)是阿里巴巴的产品，现在是[SpringCloud](https://spring.io/projects/spring-cloud)中的一个组件。相比[Eureka](https://github.com/Netflix/eureka)功能更加丰富，在国内受欢迎程度较高。

### 服务注册到nacos

​	Nacos是SpringCloudAlibaba的组件，而SpringCloudAlibaba也遵循SpringCloud中定义的服务注册、服务发现规范。因此使用Nacos和使用Eureka对于微服务来说，并没有太大区别。

主要差异在于：

- 依赖不同
- 服务地址不同

#### 1）引入依赖

在cloud-demo父工程的pom文件中的`<dependencyManagement>`中引入SpringCloudAlibaba的依赖：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.6.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

然后在user-service和order-service中的pom文件中引入nacos-discovery依赖：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

> **注意**：不要忘了注释掉eureka的依赖。

#### 2）配置nacos地址

在user-service和order-service的application.yml中添加nacos地址：

```yaml
spring:
  application:
    name: userservice  //这个服务的名称
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
```

> **注意**：不要忘了注释掉eureka的地址

#### 3）重启

重启微服务后，登录nacos管理页面（http://localhost:8848），可以看到微服务信息。



# Dubbo

​	RPC（Remote Procedure Call Protocol）远程过程调用协议。一个通俗的描述是：客户端在不知道调用细节的情况下，调用存在于远程计算机上的某个对象，就像调用本地应用程序中的对象一样。

![73701983132](img_Cloud\RPC原理图.png)

不同RPC产品的对比

![](img_Cloud\各种RPC.png)



dubbo的引用方法，

# ShardingJdbc

数据库分库分表工具



