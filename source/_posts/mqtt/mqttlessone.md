title: MQTT 折腾笔记之协议简读
date: 2013-04-25
tags: MQTT
---

##导言##

第一次听说MQTT 这玩意是由于要找个做手机推送的方案,后来发现,JPush这家伙做的实在不错,然后就不折腾了,最近,忽然心血来潮,把[MQTT 协议](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html#subscribe) 看了一遍,网上的很多**中文**的资料都是坑爹的,全部都是说MQTT 做推送,我按图索骥全部都是转载翻译自[老外2010写的这篇文章](http://tokudu.com/2010/how-to-implement-push-notifications-for-android/)实在汗颜...后来,我改用全英文关键字,搜索总算发现了MQTT 的用处.如果,你不愿意看我的长篇大论我建议你去wiki那里看下 [MQTT 详细介绍](https://en.wikipedia.org/wiki/MQ_Telemetry_Transport)
<!-- more -->
##MQTT 解决什么事情?##

对于需要要了解一个什么玩意,我们需要这玩意,解决我们什么事情.从WIKI 来看MQTT 协议主要解决的是机器与机器之间数据通信,各位想到什么没?有接触过物联网的话,可能有所了解了,当我们所有机器都能在一个网络上面分配的一个地址的话,由于,设备间的性能差异,低到可能就是一个插座,而你需要这个插座能进行数据通信,例如,控制这个插座的开-闭这类的,就需要一个极其轻量级的协议而MQTT 协议就是为此目的诞生的.

比较有趣的是,MQTT这个协议在1999 年就有了最新的版本是[v3.1(2010/12/06](http://www.ibm.com/developerworks/cn/webservices/ws-mqtt/)),其适用于如下但不限于这几点:

1. 即时传输的轻量级协议
2. 专门设计用于低带宽或者高昂的网络费用
3. 具备三种服务品质层级

##MQTT 协议简读##
MQTT 协议相对某些协议来说,实在是简短的令人发指,整个协议只用42页就说完了.

MQTT v3 到 v3.1 有几点比较重要的变化个人感觉最重要的是从ascii 码转向 utf8的支持,不过我估计没人用过v3 所有我这里不多说了,有兴趣的,请翻阅一下协议文档.....

###传输开销的比较###
MQTT 最引以为豪的就是最小的2 byte 头部传输开销.我们看下其他流行的协议的message format的设计

* [XMPP](http://www.ietf.org/rfc/rfc3920.txt) 消息体用的是xml
> `|--------------------|
   | <stream>           |
   |--------------------|
   | <presence>         |
   |   <show/>          |
   | </presence>        |
   |--------------------|
   | <message to='foo'> |
   |   <body/>          |
   | </message>         |
   |--------------------|
   | <iq to='bar'>      |
   |   <query/>         |
   | </iq>              |
   |--------------------|
   | ...                |
   |--------------------|
   | </stream>          |
   |--------------------|`

* [HTTP](https://tools.ietf.org/html/rfc2616)  
> HTTP-message   = Request | Response ; HTTP/1.1 messages

还有很多协议,就不一样细说了,就举两个我比较了解的.就目前通用的协议来看很少有比MQTT 还要低的传输开销了.如果,你有了解的希望介绍一下.

###消息体的设计简说###
<table class="bits">
	<thead>
		<tr>
			<th>bit</th>
			<th align="center">7</th>
			<th align="center">6</th>
			<th align="center">5</th>

			<th align="center">4</th>
			<th align="center">3</th>
			<th align="center">2</th>
			<th align="center">1</th>
			<th align="center">0</th>
		</tr>

	</thead>
	<tbody>
		<tr>
			<td>byte 1</td>
			<td align="center" colspan="4">Message Type</td>
			<td>DUP flag</td>
			<td align="center" colspan="2">QoS level</td>

			<td>RETAIN</td>
		</tr>
		<tr>
			<td>byte 2</td>
			<td align="center" colspan="8">Remaining Length</td>
		</tr>
	</tbody>

</table>


第一个byte 用于说明消息体的信息.

第二个byte 用于传输我们需要传输的数据.

[更多详情请看协议 msg-format 部分](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html#msg-format)

接下来,结合一个最简例子来对这个消息体进行说明

##MQTT 最简例子##
为了方便进行MQTT的了解与使用,目前MQTT的资料极其匮乏,也找不到什么给力的例子所以,随着我研究的深度,来慢慢提高这些例子的难度.

**准备**

服务端: 

* nodejs: [MQTT.js](https://github.com/adamvr/MQTT.js/)

客户端: 

* nodejs: [MQTT.js](https://github.com/adamvr/MQTT.js/)
* java: [Paho](http://www.eclipse.org/paho/) 

例子地址:[https://github.com/youxiachai/mqttlesson/tree/master/LessonOne](https://github.com/youxiachai/mqttlesson/tree/master/LessonOne) java 版本暂未提供,晚些时候写个android的客户端....


###例子设计###
为了简单,方便理解,这个例子:

1. 服务器是一个广播模型
2. 对于**订阅**/**发布**没有限制使用**topic**(主要是为了后面的知识做准备)
3. **订阅者**获取到一次**发布者**消息就断开连接



1. 首先服务端启动,接着启动 mqttClientSub

>例子流程图:  clientA ->(connect)  server

2. 启动发布者:mqttClientPub
>例子流程图:  clientB ->(publish)  server ->(pub) clientA

以上就是整个例子的流程



