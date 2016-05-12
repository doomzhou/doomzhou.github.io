---
layout: post
title: Elasticsearch集群加减节点,调配分片注意事项
categories: [Git/linux]
tags: [Ops, Elasticsearch]
---

目前我还没碰到重建索引的案例,下列记录相关增加减节点,调整复制分片

1. 首先我有2个节点,4个主分片,4个复制分片如下图


    ![](https://pbs.twimg.com/media/CiPZCa2UkAAYZdt.jpg)


2. 增加到8个复制分片


        PUT /indexname/_settings
        {
            "number_of_replicas" : 2
        }


    ![](https://pbs.twimg.com/media/CiPZCa7VEAAchec.jpg)

3. 这个时候可以增加一个节点已供新的4个复制分片


    ![](https://pbs.twimg.com/media/CiPZCa1VEAA7YFr.jpg)
    
   

4. 因为有另外两个节点有完整复制分片所以主节点可以宕机如下图
> 应该注意原先的主节点test6已经宕机，系统自动把test2选择成为master

    ![](https://pbs.twimg.com/media/CiPZCdvUgAAjTw5.jpg)

5. 删除所有的复制分片
>此时只有4个主分，两个节点，此时每个节点均有两个主分片

        PUT /blogs/_settings
        {
            "number_of_replicas" : 0
        }


    ![忘记截图了](杯具)

6. 恢复最初的那个主节点
>此时一定要注意，由于test2被选择成了master, test6启动的时修改elasticsearch.yml 中的

        discovery.zen.ping.unicast.hosts: ["test2"]

    ![忘记截图了](杯具)


    >此时由于分片数:4个主分片　3个节点


7. 接着我们怎么减少节点数呢?
>思路是:增加两份复制分片就是:4个主分片,8个复制分片,3个节点


        PUT /blogs/_settings
        {
            "number_of_replicas" : 2
        }

    > 如下图

    ![](https://pbs.twimg.com/media/CiPZMIEUgAILWoT.jpg)

    > 此时就可以继续下线一个节点,如下图


    ![](https://pbs.twimg.com/media/CiPZL_RVAAAq8_E.jpg)

    > 继续减少1份复制分片

        PUT /blogs/_settings
        {
            "number_of_replicas" : 1
        }


    ![](https://pbs.twimg.com/media/CiPZMIHUoAAjeW8.jpg)


#### 中间被人打扰了少截了个图，悲剧
