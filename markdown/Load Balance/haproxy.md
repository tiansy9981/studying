# [![img](http://docs.haproxy.org/img/HAProxyCommunityEdition_60px.png?0.4.2-12)](http://www.haproxy.org/)

## Starter Guide

**version 2.7.8-48**



```
This document doesn't provide any configuration help or hints, but it explains where to find the relevant documents. The summary below is meant to help you search sections by name and navigate through the document.

Note to documentation contributors :
    This document is formatted with 80 columns per line, with even number of
    spaces for indentation and without tabs. Please follow these rules strictly
    so that it remains easily printable everywhere. If you add sections, please
    update the summary below for easier searching.
```

# Summary

| **1.** | [**Available documentation**](http://docs.haproxy.org/2.7/intro.html#1) |      |
| ------ | ------------------------------------------------------------ | ---- |
| **2.** | [ **Quick introduction to load balancing and load balancers**](http://docs.haproxy.org/2.7/intro.html#2) |      |
| **3.** | [ **Introduction to HAProxy**](http://docs.haproxy.org/2.7/intro.html#3) |      |
| 3.1.   | [What HAProxy is and is not](http://docs.haproxy.org/2.7/intro.html#3.1) |      |
| 3.2.   | [How HAProxy works](http://docs.haproxy.org/2.7/intro.html#3.2) |      |
| 3.3.   | [Basic features](http://docs.haproxy.org/2.7/intro.html#3.3) |      |
| 3.3.1. | [Proxying](http://docs.haproxy.org/2.7/intro.html#3.3.1)     |      |
| 3.3.2. | [SSL](http://docs.haproxy.org/2.7/intro.html#3.3.2)          |      |
| 3.3.3. | [Monitoring](http://docs.haproxy.org/2.7/intro.html#3.3.3)   |      |
| 3.3.4. | [High availability](http://docs.haproxy.org/2.7/intro.html#3.3.4) |      |
| 3.3.5. | [Load balancing](http://docs.haproxy.org/2.7/intro.html#3.3.5) |      |
| 3.3.6. | [Stickiness](http://docs.haproxy.org/2.7/intro.html#3.3.6)   |      |
| 3.3.7. | [Logging](http://docs.haproxy.org/2.7/intro.html#3.3.7)      |      |
| 3.3.8. | [Statistics](http://docs.haproxy.org/2.7/intro.html#3.3.8)   |      |
| 3.4.   | [Standard features](http://docs.haproxy.org/2.7/intro.html#3.4) |      |
| 3.4.1. | [Sampling and converting information](http://docs.haproxy.org/2.7/intro.html#3.4.1) |      |
| 3.4.2. | [Maps](http://docs.haproxy.org/2.7/intro.html#3.4.2)         |      |
| 3.4.3. | [ACLs and conditions](http://docs.haproxy.org/2.7/intro.html#3.4.3) |      |
| 3.4.4. | [Content switching](http://docs.haproxy.org/2.7/intro.html#3.4.4) |      |
| 3.4.5. | [Stick-tables](http://docs.haproxy.org/2.7/intro.html#3.4.5) |      |
| 3.4.6. | [Formatted strings](http://docs.haproxy.org/2.7/intro.html#3.4.6) |      |
| 3.4.7. | [HTTP rewriting and redirection](http://docs.haproxy.org/2.7/intro.html#3.4.7) |      |
| 3.4.8. | [Server protection](http://docs.haproxy.org/2.7/intro.html#3.4.8) |      |
| 3.5.   | [Advanced features](http://docs.haproxy.org/2.7/intro.html#3.5) |      |
| 3.5.1. | [Management](http://docs.haproxy.org/2.7/intro.html#3.5.1)   |      |
| 3.5.2. | [System-specific capabilities](http://docs.haproxy.org/2.7/intro.html#3.5.2) |      |
| 3.5.3. | [Scripting](http://docs.haproxy.org/2.7/intro.html#3.5.3)    |      |
| 3.5.4. | [Advanced features: Tracing](http://docs.haproxy.org/2.7/intro.html#3.5.4) |      |
| 3.6.   | [Sizing](http://docs.haproxy.org/2.7/intro.html#3.6)         |      |
| 3.7.   | [How to get HAProxy](http://docs.haproxy.org/2.7/intro.html#3.7) |      |
| **4.** | [ **Companion products and alternatives**](http://docs.haproxy.org/2.7/intro.html#4) |      |
| 4.1.   | [Apache HTTP server](http://docs.haproxy.org/2.7/intro.html#4.1) |      |
| 4.2.   | [NGINX](http://docs.haproxy.org/2.7/intro.html#4.2)          |      |
| 4.3.   | [Varnish](http://docs.haproxy.org/2.7/intro.html#4.3)        |      |
| 4.4.   | [Alternatives](http://docs.haproxy.org/2.7/intro.html#4.4)   |      |
| **5.** | [ **Contacts**](http://docs.haproxy.org/2.7/intro.html#5)    |      |



# [1.](http://docs.haproxy.org/2.7/intro.html#1) Available documentation

```
The complete HAProxy documentation is contained in the following documents.Please ensure to consult the relevant documentation to save time and to get themost accurate response to your needs. Also please refrain from sending questionsto the mailing list whose responses are present in these documents.

  - intro.txt (this document) : it presents the basics of load balancing,HAProxy as a product, what it does, what it doesn't do, some known traps to avoid, some OS-specific limitations, how to get it, how it evolves, how to ensure you're running with all known fixes, how to update it, complements and alternatives.

  - management.txt : it explains how to start haproxy, how to manage it at runtime, how to manage it on multiple nodes, and how to proceed with seamless upgrades.

  - configuration.txt : the reference manual details all configuration keywords and their options. It is used when a configuration change is needed.

  - coding-style.txt : this is for developers who want to propose some code to the project. It explains the style to adopt for the code. It is not very strict and not all the code base completely respects it, but contributions which diverge too much from it will be rejected.

  - proxy-protocol.txt : this is the de-facto specification of the PROXY protocol which is implemented by HAProxy and a number of third party products.

  - README : how to build HAProxy from sources
```



# [2.](http://docs.haproxy.org/2.7/intro.html#2) Quick introduction to load balancing and load balancers

```
Load balancing consists in aggregating multiple components in order to achieve a total processing capacity above each component's individual capacity, without any intervention from the end user and in a scalable way. This results in more operations being performed simultaneously by the time it takes a component to perform only one. A single operation however will still be performed on a single component at a time and will not get faster than without load balancing. It always requires at least as many operations as available components and an efficient load balancing mechanism to make use of all components and to fully benefit from the load balancing. A good example of this is the number of lanes on a highway which allows as many cars to pass during the same time frame without increasing their individual speed.
负载平衡包括聚合多个组件，以实现超过每个组件单独容量的总处理能力，而不需要最终用户的任何干预，并且以可扩展的方式进行。这导致在组件只执行一个操作的时间内同时执行了更多操作。然而，单个操作一次仍将在单个组件上执行，并且不会比没有负载平衡时更快。它总是需要至少与可用组件一样多的操作和有效的负载平衡机制来利用所有组件并充分受益于负载平衡。一个很好的例子是高速公路上的车道数，它允许在同一时间段内尽可能多的汽车通过，而不增加他们的个人速度。

Examples of load balancing :

  - Process scheduling in multi-processor systems
  - Link load balancing (e.g. EtherChannel, Bonding)
  - IP address load balancing (e.g. ECMP, DNS round-robin)
  - Server load balancing (via load balancers)

The mechanism or component which performs the load balancing operation is called a load balancer. In web environments these components are called a "network load balancer", and more commonly a "load balancer" given that this activity is by far the best known case of load balancing.
负载平衡的例子:

-多处理器系统中的进程调度
—链路负载均衡(如EtherChannel、Bonding)
—IP地址负载均衡(如ECMP、DNS轮询)
-服务器负载平衡(通过负载平衡器)

执行负载平衡操作的机制或组件称为负载平衡器。在web环境中，这些组件被称为“网络负载平衡器”，更常见的是“负载平衡器”，因为这种活动是迄今为止最著名的负载平衡案例。

A load balancer may act :

  - at the link level : this is called link load balancing, and it consists in choosing what network link to send a packet to;

  - at the network level : this is called network load balancing, and it consists in choosing what route a series of packets will follow;

  - at the server level : this is called server load balancing and it consists in deciding what server will process a connection or request.

Two distinct technologies exist and address different needs, though with some overlapping. In each case it is important to keep in mind that load balancing consists in diverting the traffic from its natural flow and that doing so always requires a minimum of care to maintain the required level of consistency between all routing decisions.

负载均衡器可以这样做:

-在链路级别:这被称为链路负载平衡，它包括选择将数据包发送到哪个网络链路;

-在网络层面:这被称为网络负载平衡，它包括选择一系列数据包将遵循的路由;

-在服务器层面:这被称为服务器负载平衡，它包括决定哪台服务器将处理连接或请求。

存在两种不同的技术，它们满足不同的需求，尽管有一些重叠。在每种情况下，重要的是要记住，负载平衡包括将流量从其自然流中转移出来，并且这样做总是需要最少的注意来保持所有路由决策之间所需的一致性级别。

The first one acts at the packet level and processes packets more or less individually. There is a 1-to-1 relation between input and output packets, so it is possible to follow the traffic on both sides of the load balancer using a regular network sniffer. This technology can be very cheap and extremely fast. 
It is usually implemented in hardware (ASICs) allowing to reach line rate, such as switches doing ECMP. Usually stateless, it can also be stateful (consider the session a packet belongs to and called layer4-LB or L4), may support DSR (direct server return, without passing through the LB again) if the packets were not modified, but provides almost no content awareness. This technology is very well suited to network-level load balancing, though it is sometimes used for very basic server load balancing at high speed.

The second one acts on session contents. It requires that the input streams is reassembled and processed as a whole. The contents may be modified, and the output stream is segmented into new packets. For this reason it is generally performed by proxies and they're often called layer 7 load balancers or L7. This implies that there are two distinct connections on each side, and that there is no relation between input and output packets sizes nor counts. Clients and servers are not required to use the same protocol (for example IPv4 vs IPv6, clear vs SSL). The operations are always stateful, and the return traffic must pass through the load balancer. The extra processing comes with a cost so it's not always possible to achieve line rate, especially with small packets. On the other hand, it offers wide possibilities and is generally achieved by pure software, even if embedded into hardware appliances. This technology is very well suited for server load balancing.

Packet-based load balancers are generally deployed in cut-through mode, so they are installed on the normal path of the traffic and divert it according to the configuration. The return traffic doesn't necessarily pass through the load balancer. Some modifications may be applied to the network destination address in order to direct the traffic to the proper destination. In this case, it is mandatory that the return traffic passes through the load balancer. If the routes doesn't make this possible, the load balancer may also replace the packets' source address with its own in order to force the return traffic to pass through it.

第一个在数据包级别起作用，并或多或少地单独处理数据包。输入和输出数据包之间存在1对1的关系，因此可以使用常规网络嗅探器跟踪负载平衡器两侧的流量。这项技术非常便宜，而且速度极快。
它通常在硬件(asic)中实现，允许达到线路速率，例如执行ECMP的交换机。通常是无状态的，它也可以是有状态的(考虑数据包所属的会话，称为layer4-LB或L4)，如果数据包没有被修改，它可能支持DSR(直接服务器返回，而不再次经过LB)，但几乎不提供内容感知。这种技术非常适合于网络级负载平衡，尽管它有时用于非常基本的高速服务器负载平衡。

第二个函数作用于会话内容。它要求输入流作为一个整体重新组装和处理。内容可能被修改，输出流被分割成新的报文。由于这个原因，它通常由代理执行，它们通常被称为第7层负载平衡器或L7。这意味着每一侧有两个不同的连接，并且输入和输出数据包大小和计数之间没有关系。客户端和服务器不需要使用相同的协议(例如IPv4 vs IPv6, clear vs SSL)。操作总是有状态的，返回的流量必须经过负载平衡器。额外的处理是有成本的，所以不可能总是达到线路速率，特别是对于小数据包。另一方面，它提供了广泛的可能性，并且通常由纯软件实现，即使嵌入到硬件设备中。这种技术非常适合服务器负载平衡。

基于包的负载均衡器一般采用直通方式部署，安装在流量的正常路径上，根据配置进行分流。返回的流量不一定要经过负载平衡器。为了将流量引导到正确的目的地，可以对网络目的地址进行一些修改。在这种情况下，返回流量必须经过负载均衡器。如果路由不能实现这一点，负载均衡器也可以用自己的源地址替换数据包的源地址，以强制返回流量通过它。
 

Proxy-based load balancers are deployed as a server with their own IP addresses and ports, without architecture changes. Sometimes this requires to perform some adaptations to the applications so that clients are properly directed to the load balancer's IP address and not directly to the server's. Some load balancers may have to adjust some servers' responses to make this possible (e.g. the HTTP Location header field used in HTTP redirects). Some proxy-based load balancers may intercept traffic for an address they don't own, and spoof the client's address when connecting to the server. This allows them to be deployed as if they were a regular router or firewall, in a cut-through mode very similar to the packet based load balancers. This is particularly appreciated for products which combine both packet mode and proxy mode. In this case DSR is obviously still not possible and the return traffic still has to be routed back to the load balancer.

A very scalable layered approach would consist in having a front router which receives traffic from multiple load balanced links, and uses ECMP to distribute this traffic to a first layer of multiple stateful packet-based load balancers (L4). These L4 load balancers in turn pass the traffic to an even larger number of proxy-based load balancers (L7), which have to parse the contents to decide what server will ultimately receive the traffic.

The number of components and possible paths for the traffic increases the risk of failure; in very large environments, it is even normal to permanently have a few faulty components being fixed or replaced. Load balancing done without awareness of the whole stack's health significantly degrades availability. For this reason, any sane load balancer will verify that the components it intends to deliver the traffic to are still alive and reachable, and it will stop delivering traffic to faulty ones. This can be achieved using various methods.

The most common one consists in periodically sending probes to ensure the component is still operational. These probes are called "health checks". They must be representative of the type of failure to address. For example a ping-based check will not detect that a web server has crashed and doesn't listen to a port anymore, while a connection to the port will verify this, and a more advanced request may even validate that the server still works and that the database it relies on is still accessible. Health checks often involve a few retries to cover for occasional measuring errors. The period between checks must be small enough to ensure the faulty component is not used for too long after an error occurs.

基于代理的负载平衡器部署为具有自己IP地址和端口的服务器，无需更改体系结构。有时，这需要对应用程序进行一些调整，以便将客户端正确定向到负载平衡器的IP地址，而不是直接定向到服务器的IP地址。一些负载均衡器可能需要调整一些服务器的响应来实现这一点(例如，HTTP重定向中使用的HTTP Location头字段)。一些基于代理的负载平衡器可能会拦截他们不拥有的地址的流量，并在连接到服务器时欺骗客户端的地址。这使得它们可以像普通路由器或防火墙一样进行部署，采用与基于数据包的负载均衡器非常相似的直通模式。这对于结合了包模式和代理模式的产品特别有用。在这种情况下，DSR显然仍然是不可能的，返回的流量仍然必须路由回负载均衡器。

一种非常可扩展的分层方法包括有一个前端路由器，它接收来自多个负载平衡链路的流量，并使用ECMP将此流量分发到多个基于状态数据包的负载平衡器(L4)的第一层。这些L4负载平衡器依次将流量传递给数量更多的基于代理的负载平衡器(L7)， L7必须解析内容以决定最终接收流量的服务器。

组件的数量和流量的可能路径增加了故障的风险;在非常大的环境中，永久性地修复或更换一些有故障的组件甚至是正常的。在不了解整个堆栈运行状况的情况下进行负载平衡会显著降低可用性。出于这个原因，任何负载平衡器都将验证它打算交付流量的组件仍然是活动的和可到达的，并且它将停止向有故障的组件交付流量。这可以通过各种方法来实现。

最常见的方法是定期发送探测，以确保组件仍可运行。这些探针被称为“健康检查”。它们必须是要解决的故障类型的代表。例如，基于ping的检查将不会检测到web服务器已经崩溃并且不再侦听端口，而连接到端口将验证这一点，并且更高级的请求甚至可能验证服务器仍然工作并且它所依赖的数据库仍然是可访问的。运行状况检查通常需要进行几次重试，以弥补偶尔出现的测量错误。检查之间的间隔时间必须足够短，以确保故障组件在发生错误后不会使用太长时间。
 

Other methods consist in sampling the production traffic sent to a destination to observe if it is processed correctly or not, and to evict the components which return inappropriate responses. However this requires to sacrifice a part of the production traffic and this is not always acceptable. A combination of these two mechanisms provides the best of both worlds, with both of them being used to detect a fault, and only health checks to detect the end of the fault. A last method involves centralized reporting : a central monitoring agent periodically updates all load balancers about all components' state. This gives a global view of the infrastructure to all components, though sometimes with less accuracy or responsiveness. It's best suited for environments with many load balancers and many servers.

Layer 7 load balancers also face another challenge known as stickiness or persistence. The principle is that they generally have to direct multiple subsequent requests or connections from a same origin (such as an end user) to the same target. The best known example is the shopping cart on an online store. If each click leads to a new connection, the user must always be sent to the server which holds his shopping cart. Content-awareness makes it easier to spot some elements in the request to identify the server to deliver it to, but that's not always enough. For example if the source address is used as a key to pick a server, it can be decided that a hash-based algorithm will be used and that a given IP address will always be sent to the same server based on a divide of the address by the number of available servers. But if one server fails, the result changes and all users are suddenly sent to a different server and lose their shopping cart. The solution against this issue consists in memorizing the chosen target so that each time the same visitor is seen,he's directed to the same server regardless of the number of available servers.The information may be stored in the load balancer's memory, in which case it may have to be replicated to other load balancers if it's not alone, or it may be stored in the client's memory using various methods provided that the client is able to present this information back with every request (cookie insertion,redirection to a sub-domain, etc). This mechanism provides the extra benefit of not having to rely on unstable or unevenly distributed information (such as the source IP address). This is in fact the strongest reason to adopt a layer 7 load balancer instead of a layer 4 one.

其他方法包括对发送到目的地的生产流量进行采样，以观察它是否被正确处理，并驱逐返回不适当响应的组件。然而，这需要牺牲一部分生产流量，这并不总是可以接受的。这两种机制的组合提供了两全其美的效果，它们都用于检测故障，而仅使用健康检查来检测故障的结束。最后一种方法涉及集中报告:中央监视代理定期更新所有组件状态的所有负载平衡器。这为所有组件提供了基础设施的全局视图，尽管有时准确性或响应性较低。它最适合具有许多负载平衡器和许多服务器的环境。

第7层负载平衡器还面临另一个挑战，即粘性或持久性。其原则是，它们通常必须将来自相同来源(例如最终用户)的多个后续请求或连接定向到相同的目标。最著名的例子是在线商店的购物车。如果每次点击都会导致一个新的连接，那么必须总是将用户发送到保存其购物车的服务器。内容感知可以更容易地发现请求中的某些元素，从而确定要将其传递到的服务器，但这并不总是足够的。例如，如果源地址用作选择服务器的密钥，则可以决定使用基于散列的算法，并且根据地址除以可用服务器的数量，将给定的IP地址始终发送到同一服务器。但是，如果一个服务器出现故障，结果就会发生变化，所有用户都会突然被发送到另一个服务器，并丢失他们的购物车。针对此问题的解决方案包括记住所选的目标，以便每次看到相同的访问者时，无论可用服务器的数量如何，都将其定向到相同的服务器。信息可能存储在负载均衡器的内存中，在这种情况下，如果它不是单独的，它可能必须复制到其他负载均衡器中，或者它可能使用各种方法存储在客户端内存中，前提是客户端能够在每个请求(cookie插入，重定向到子域等)中呈现此信息。这种机制提供了额外的好处，不必依赖于不稳定或不均匀分布的信息(如源IP地址)。这实际上是采用第7层负载均衡器而不是第4层负载均衡器的最有力的理由。
 


In order to extract information such as a cookie, a host header field, a URL or whatever, a load balancer may need to decrypt SSL/TLS traffic and even possibly to re-encrypt it when passing it to the server. This expensive task explains why in some high-traffic infrastructures, sometimes there may be a lot of load balancers.

Since a layer 7 load balancer may perform a number of complex operations on the traffic (decrypt, parse, modify, match cookies, decide what server to send to, etc), it can definitely cause some trouble and will very commonly be accused of being responsible for a lot of trouble that it only revealed. Often it will be discovered that servers are unstable and periodically go up and down, or for web servers, that they deliver pages with some hard-coded links forcing the clients to connect directly to one specific server without passing via the load balancer, or that they take ages to respond under high load causing timeouts.That's why logging is an extremely important aspect of layer 7 load balancing.Once a trouble is reported, it is important to figure if the load balancer took a wrong decision and if so why so that it doesn't happen anymore.


为了提取诸如cookie、主机报头字段、URL或其他信息，负载均衡器可能需要解密SSL/TLS流量，甚至可能在将其传递给服务器时重新加密。这个昂贵的任务解释了为什么在一些高流量基础设施中，有时可能会有很多负载平衡器。

由于第7层负载均衡器可能对流量执行许多复杂的操作(解密，解析，修改，匹配cookie，决定发送到哪个服务器等)，它肯定会造成一些麻烦，并且通常会被指责为对许多麻烦负责，它只是揭示了。通常会发现服务器不稳定，并且会周期性地上下波动，或者对于web服务器，它们提供的页面带有一些硬编码链接，迫使客户端直接连接到一个特定的服务器，而不通过负载平衡器，或者它们在高负载下需要很长时间才能响应，导致超时。这就是为什么日志记录是第7层负载平衡的一个极其重要的方面。一旦报告了问题，重要的是要弄清楚负载平衡器是否做出了错误的决定，如果是的话，为什么要避免再发生错误。
 
```



# [3.](http://docs.haproxy.org/2.7/intro.html#3) Introduction to HAProxy

```
HAProxy is written as "HAProxy" to designate the product, and as "haproxy" todesignate the executable program, software package or a process. However, both are commonly used for both purposes, and are pronounced H-A-Proxy. Very early, "haproxy" used to stand for "high availability proxy" and the name was written in two separate words, though by now it means nothing else than "HAProxy".


HAProxy用“HAProxy”表示产品，用“HAProxy”表示可执行程序、软件包或进程。然而，两者通常用于两种目的，并且发音为H-A-Proxy。很早就，“haproxy”代表“高可用性代理”，它的名字是用两个单独的单词写成的，尽管现在它的意思就是“haproxy”。
```



## [3.1.](http://docs.haproxy.org/2.7/intro.html#3.1) What HAProxy is and isn't

```
HAProxy is :

  - a TCP proxy : it can accept a TCP connection from a listening socket,connect to a server and attach these sockets together allowing traffic to flow in both directions; IPv4, IPv6 and even UNIX sockets are supported on either side, so this can provide an easy way to translate addresses between different families.

  - an HTTP reverse-proxy (called a "gateway" in HTTP terminology) : it presents itself as a server, receives HTTP requests over connections accepted on a listening TCP socket, and passes the requests from these connections to servers using different connections. It may use any combination of HTTP/1.x or HTTP/2 on any side and will even automatically detect the protocol spoken on each side when ALPN is used over TLS.

  - an SSL terminator / initiator / offloader : SSL/TLS may be used on the  connection coming from the client, on the connection going to the server,or even on both connections. A lot of settings can be applied per name(SNI), and may be updated at runtime without restarting. Such setups are extremely scalable and deployments involving tens to hundreds of thousands of certificates were reported.

  - a TCP normalizer : since connections are locally terminated by the operating system, there is no relation between both sides, so abnormal traffic such as invalid packets, flag combinations, window advertisements, sequence numbers,incomplete connections (SYN floods), or so will not be passed to the other side. This protects fragile TCP stacks from protocol attacks, and also allows to optimize the connection parameters with the client without having to modify the servers' TCP stack settings.

  - an HTTP normalizer : when configured to process HTTP traffic, only valid complete requests are passed. This protects against a lot of protocol-based attacks. Additionally, protocol deviations for which there is a tolerance in the specification are fixed so that they don't cause problem on the servers (e.g. multiple-line headers).

  - an HTTP fixing tool : it can modify / fix / add / remove / rewrite the URL or any request or response header. This helps fixing interoperability issues in complex environments.
  
 HAProxy是:

-一个TCP代理:它可以接受一个TCP连接从监听套接字，连接到服务器和附加这些套接字在一起，允许流量在两个方向上流动;IPv4、IPv6甚至UNIX套接字都支持，因此这可以提供一种在不同家族之间转换地址的简单方法。

- HTTP反向代理(在HTTP术语中称为“网关”):它将自己呈现为服务器，通过侦听TCP套接字接受的连接接收HTTP请求，并将这些连接的请求传递给使用不同连接的服务器。它可以使用HTTP/1的任何组合。当在TLS上使用ALPN时，它甚至会自动检测每一方所说的协议。

SSL终止器/启动器/卸载器:SSL/TLS可以在来自客户端的连接上使用，也可以在去往服务器的连接上使用，甚至在两个连接上都使用。许多设置可以应用于每个名称(SNI)，并且可以在运行时更新，而无需重新启动。这样的设置具有极大的可扩展性，并且报告了涉及数万到数十万个证书的部署。

- TCP规范化:由于连接是由操作系统在本地终止的，因此双方之间没有关系，因此诸如无效数据包，标志组合，窗口广告，序列号，不完整连接(SYN洪水)等异常流量不会传递到另一端。这可以保护脆弱的TCP堆栈免受协议攻击，并且还允许在不修改服务器的TCP堆栈设置的情况下与客户端优化连接参数。

- HTTP规范化:当配置为处理HTTP流量时，只传递有效的完整请求。这可以防止许多基于协议的攻击。此外，规范中允许的协议偏差是固定的，这样它们就不会在服务器上造成问题(例如多行标头)。

-一个HTTP修复工具:它可以修改/修复/添加/删除/重写URL或任何请求或响应头。这有助于解决复杂环境中的互操作性问题。
  
  

  - a content-based switch : it can consider any element from the request to decide what server to pass the request or connection to. Thus it is possible to handle multiple protocols over a same port (e.g. HTTP, HTTPS, SSH).

  - a server load balancer : it can load balance TCP connections and HTTP requests. In TCP mode, load balancing decisions are taken for the whole connection. In HTTP mode, decisions are taken per request.

  - a traffic regulator : it can apply some rate limiting at various points, protect the servers against overloading, adjust traffic priorities based on  the contents, and even pass such information to lower layers and outer  network components by marking packets.

  - a protection against DDoS and service abuse : it can maintain a wide number of statistics per IP address, URL, cookie, etc and detect when an abuse is happening, then take action (slow down the offenders, block them, send them to outdated contents, etc).

  - an observation point for network troubleshooting : due to the precision of the information reported in logs, it is often used to narrow down some  network-related issues.

  - an HTTP compression offloader : it can compress responses which were not compressed by the server, thus reducing the page load time for clients with  poor connectivity or using high-latency, mobile networks.

  - a caching proxy : it may cache responses in RAM so that subsequent requests  for the same object avoid the cost of another network transfer from the  server as long as the object remains present and valid. It will however not  store objects to any persistent storage. Please note that this caching  feature is designed to be maintenance free and focuses solely on saving haproxy's precious resources and not on save the server's resources. Caches designed to optimize servers require much more tuning and flexibility. If  you instead need such an advanced cache, please use Varnish Cache, which integrates perfectly with haproxy, especially when SSL/TLS is needed on any side.

  - a FastCGI gateway : FastCGI can be seen as a different representation of HTTP, and as such, HAProxy can directly load-balance a farm comprising any combination of FastCGI application servers without requiring to insert another level of gateway between them. This results in resource savings and a reduction of maintenance costs.
  
-基于内容的交换机:它可以考虑请求中的任何元素来决定将请求或连接传递到哪个服务器。因此，可以在同一个端口上处理多个协议(例如HTTP、HTTPS、SSH)。

—服务器负载均衡器:对TCP连接和HTTP请求进行负载均衡。在TCP模式下，对整个连接进行负载平衡决策。在HTTP模式中，每个请求都做出决定。

-流量调节器:它可以在各个点上应用一些速率限制，保护服务器不过载，根据内容调整流量优先级，甚至通过标记数据包将这些信息传递给较低层次和外部网络组件。

-针对DDoS和服务滥用的保护:它可以维护每个IP地址，URL, cookie等的大量统计数据，并检测何时发生滥用，然后采取行动(减慢罪犯，阻止他们，将他们发送到过时的内容等)。

—网络故障的观察点:由于日志中所报告信息的精确性，通常用于缩小一些网络问题的范围。

-一个HTTP压缩卸载器:它可以压缩响应，没有被服务器压缩，从而减少页面加载时间客户端连接差或使用高延迟，移动网络。

-缓存代理:它可以在RAM中缓存响应，以便后续对同一对象的请求避免从服务器进行另一次网络传输的成本，只要对象仍然存在且有效。但是，它不会将对象存储到任何持久存储中。请注意，这个缓存功能是为了免维护而设计的，并且只关注于节省haproxy的宝贵资源，而不是节省服务器的资源。为优化服务器而设计的缓存需要更多的调优和灵活性。如果你需要这样的高级缓存，请使用Varnish缓存，它与haproxy完美集成，特别是当任何一方需要SSL/TLS时。

FastCGI网关:FastCGI可以看作是HTTP的一种不同的表示，因此，HAProxy可以直接负载平衡一个由任意FastCGI应用服务器组成的farm，而不需要在它们之间插入另一个级别的网关。这样可以节省资源并降低维护成本。
 
  

HAProxy is not :

  - an explicit HTTP proxy, i.e. the proxy that browsers use to reach the internet. There are excellent open-source software dedicated for this task,such as Squid. However HAProxy can be installed in front of such a proxy to provide load balancing and high availability.

  - a data scrubber : it will not modify the body of requests nor responses.

  - a static web server : during startup, it isolates itself inside a chroot jail and drops its privileges, so that it will not perform any single file-system access once started. As such it cannot be turned into a static web server (dynamic servers are supported through FastCGI however). There are excellent open-source software for this such as Apache or Nginx, and HAProxy can be easily installed in front of them to provide load balancing,high availability and acceleration.

  - a packet-based load balancer : it will not see IP packets nor UDP datagrams,will not perform NAT or even less DSR. These are tasks for lower layers.
    Some kernel-based components such as IPVS (Linux Virtual Server) already do this pretty well and complement perfectly with HAProxy.
    
  HAProxy不是:

-显式HTTP代理，即浏览器用来访问互联网的代理。有一些优秀的开源软件专门用于这项任务，比如Squid。然而，HAProxy可以安装在这样一个代理的前面，以提供负载平衡和高可用性。

-数据清除器:它不会修改请求和响应的主体。

-静态web服务器:在启动期间，它将自己隔离在chroot jail中并放弃其特权，因此一旦启动，它将不会执行任何单个文件系统访问。因此，它不能变成一个静态web服务器(动态服务器是通过FastCGI支持的)。有很好的开源软件，如Apache或Nginx, HAProxy可以很容易地安装在它们前面，以提供负载平衡，高可用性和加速。

-一个基于包的负载均衡器:它不会看到IP数据包和UDP数据报，不会执行NAT甚至更少的DSR。这些是低层的任务。
一些基于内核的组件，如IPVS (Linux Virtual Server)已经在这方面做得很好，并与HAProxy完美互补。
 
```



## [3.2.](http://docs.haproxy.org/2.7/intro.html#3.2) How HAProxy works

```
HAProxy is an event-driven, non-blocking engine combining a very fast I/O layer with a priority-based, multi-threaded scheduler. As it is designed with a data forwarding goal in mind, its architecture is optimized to move data as fast as possible with the least possible operations. It focuses on optimizing the CPU cache's efficiency by sticking connections to the same CPU as long as possible.
As such it implements a layered model offering bypass mechanisms at each level ensuring data doesn't reach higher levels unless needed. Most of the processing is performed in the kernel, and HAProxy does its best to help the kernel do the work as fast as possible by giving some hints or by avoiding certain operation when it guesses they could be grouped later. As a result, typical figures show 15% of the processing time spent in HAProxy versus 85% in the kernel in TCP or HTTP close mode, and about 30% for HAProxy versus 70% for the kernel in HTTP keep-alive mode.

A single process can run many proxy instances; configurations as large as 300000 distinct proxies in a single process were reported to run fine. A single core, single CPU setup is far more than enough for more than 99% users, and as such, users of containers and virtual machines are encouraged to use the absolute smallest images they can get to save on operational costs and simplify troubleshooting. However the machine HAProxy runs on must never ever swap, and its CPU must not be artificially throttled (sub-CPU allocation in hypervisors) nor be shared with compute-intensive processes which would induce a very high context-switch latency.

Threading allows to exploit all available processing capacity by using one thread per CPU core. This is mostly useful for SSL or when data forwarding rates above 40 Gbps are needed. In such cases it is critically important to avoid communications between multiple physical CPUs, which can cause strong bottlenecks in the network stack and in HAProxy itself. While counter-intuitive to some, the first thing to do when facing some performance issues is often to reduce the number of CPUs HAProxy runs on.

HAProxy only requires the haproxy executable and a configuration file to run. For logging it is highly recommended to have a properly configured syslog daemon and log rotations in place. Logs may also be sent to stdout/stderr, which can be useful inside containers. The configuration files are parsed before starting, then HAProxy tries to bind all listening sockets, and refuses to start if anything fails. Past this point it cannot fail anymore. This means that there are no runtime failures and that if it accepts to start, it will work until it is stopped.

HAProxy是一个事件驱动的非阻塞引擎，它结合了一个非常快速的I/O层和一个基于优先级的多线程调度器。由于在设计时考虑了数据转发目标，因此对其架构进行了优化，以尽可能少的操作尽可能快地移动数据。它侧重于通过尽可能长时间地将连接固定到同一个CPU来优化CPU缓存的效率。
因此，它实现了一个分层模型，在每个级别提供绕过机制，确保除非需要，否则数据不会到达更高的级别。大多数处理是在内核中执行的，HAProxy尽其所能帮助内核尽可能快地完成工作，方法是给出一些提示，或者在内核猜测某些操作可能稍后进行分组时避免这些操作。因此，典型的数据显示，在TCP或HTTP关闭模式下，HAProxy的处理时间占15%，而内核的处理时间占85%;在HTTP保持活动模式下，HAProxy的处理时间占30%，内核的处理时间占70%。

单个进程可以运行多个代理实例;据报告，单个进程中多达300000个不同代理的配置运行良好。对于99%以上的用户来说，单个核心、单个CPU设置已经足够了，因此，鼓励容器和虚拟机的用户使用尽可能小的映像，以节省操作成本并简化故障排除。但是，HAProxy运行的机器永远不能交换，它的CPU不能被人为地限制(管理程序中的子CPU分配)，也不能与计算密集型进程共享，否则会导致非常高的上下文切换延迟。

线程允许通过每个CPU内核使用一个线程来利用所有可用的处理能力。这对于SSL或需要超过40 Gbps的数据转发速率时非常有用。在这种情况下，避免多个物理cpu之间的通信是非常重要的，这可能会导致网络堆栈和HAProxy本身出现严重瓶颈。虽然对某些人来说这是违反直觉的，但当面临某些性能问题时，首先要做的事情通常是减少HAProxy运行的cpu数量。

HAProxy只需要HAProxy可执行文件和配置文件即可运行。对于日志记录，强烈建议配置适当的syslog守护进程和日志循环。日志也可以发送到stdout/stderr，这在容器中很有用。在启动之前解析配置文件，然后HAProxy尝试绑定所有侦听套接字，如果有任何失败则拒绝启动。过了这一点，它就不能再失败了。这意味着没有运行时失败，如果它接受启动，它将一直工作直到停止。
 


Once HAProxy is started, it does exactly 3 things :

  - process incoming connections;

  - periodically check the servers' status (known as health checks);

  - exchange information with other haproxy nodes.

Processing incoming connections is by far the most complex task as it depends on a lot of configuration possibilities, but it can be summarized as the 9 steps below :

  - accept incoming connections from listening sockets that belong to a configuration entity known as a "frontend", which references one or multiple listening addresses;

  - apply the frontend-specific processing rules to these connections that may result in blocking them, modifying some headers, or intercepting them to execute some internal applets such as the statistics page or the CLI;

  - pass these incoming connections to another configuration entity representing a server farm known as a "backend", which contains the list of servers and the load balancing strategy for this server farm;

  - apply the backend-specific processing rules to these connections;

  - decide which server to forward the connection to according to the load balancing strategy;

  - apply the backend-specific processing rules to the response data;

  - apply the frontend-specific processing rules to the response data;

  - emit a log to report what happened in fine details;

  - in HTTP, loop back to the second step to wait for a new request, otherwise close the connection.

一旦HAProxy启动，它会做三件事:

-处理传入连接;

-定期检查服务器的状态(称为健康检查);

-与其他haproxy节点交换信息。

处理传入连接是迄今为止最复杂的任务，因为它取决于许多配置可能性，但它可以总结为以下9个步骤:

-接受来自侦听套接字的连接，这些套接字属于一个被称为“前端”的配置实体，它引用一个或多个侦听地址;

-应用前端特定的处理规则，这些连接可能会导致阻塞，修改一些头，或拦截他们执行一些内部小程序，如统计页面或CLI;

-将这些传入的连接传递到另一个配置实体，代表一个服务器场，称为“后端”，其中包含服务器列表和该服务器场的负载均衡策略;

-对这些连接应用特定于后端的处理规则;

-根据负载均衡策略决定将连接转发到哪个服务器;

-对响应数据应用特定于后台的处理规则;

-对响应数据应用前端特定处理规则;

-发出日志，详细报告发生的事情;

-在HTTP中，循环回第二步等待新请求，否则关闭连接。
 


Frontends and backends are sometimes considered as half-proxies, since they only look at one side of an end-to-end connection; the frontend only cares about the clients while the backend only cares about the servers. HAProxy also supports full proxies which are exactly the union of a frontend and a backend. When HTTP processing is desired, the configuration will generally be split into frontends and backends as they open a lot of possibilities since any frontend may pass a connection to any backend. With TCP-only proxies, using frontends and backends rarely provides a benefit and the configuration can be more readable with full proxies.

前端和后端有时被认为是半代理，因为它们只查看端到端连接的一端;前端只关心客户端，而后端只关心服务器。HAProxy还支持完全代理，即前端和后端的联合。当需要HTTP处理时，配置通常会分为前端和后端，因为它们提供了很多可能性，因为任何前端都可以将连接传递到任何后端。使用纯tcp代理时，使用前端和后端很少有好处，使用完整代理时配置更容易读懂。
```



## [3.3.](http://docs.haproxy.org/2.7/intro.html#3.3) Basic features

```
This section will enumerate a number of features that HAProxy implements, some of which are generally expected from any modern load balancer, and some of which are a direct benefit of HAProxy's architecture. More advanced features will be detailed in the next section.

本节将列举HAProxy实现的许多特性，其中一些通常是任何现代负载平衡器所期望的，其中一些是HAProxy架构的直接好处。下一节将详细介绍更高级的特性。
```



### [3.3.1.](http://docs.haproxy.org/2.7/intro.html#3.3.1) Basic features : Proxying

```
Proxying is the action of transferring data between a client and a server over two independent connections. The following basic features are supported by HAProxy regarding proxying and connection management :

  - Provide the server with a clean connection to protect them against any client-side defect or attack;

  - Listen to multiple IP addresses and/or ports, even port ranges;

  - Transparent accept : intercept traffic targeting any arbitrary IP address that doesn't even belong to the local system;

  - Server port doesn't need to be related to listening port, and may even be translated by a fixed offset (useful with ranges);

  - Transparent connect : spoof the client's (or any) IP address if needed when connecting to the server;

  - Provide a reliable return IP address to the servers in multi-site LBs;

  - Offload the server thanks to buffers and possibly short-lived connections to reduce their concurrent connection count and their memory footprint;

  - Optimize TCP stacks (e.g. SACK), congestion control, and reduce RTT impacts;

  - Support different protocol families on both sides (e.g. IPv4/IPv6/Unix);

  - Timeout enforcement : HAProxy supports multiple levels of timeouts depending on the stage the connection is, so that a dead client or server, or an attacker cannot be granted resources for too long;

  - Protocol validation: HTTP, SSL, or payload are inspected and invalid protocol elements are rejected, unless instructed to accept them anyway;

  - Policy enforcement : ensure that only what is allowed may be forwarded;

  - Both incoming and outgoing connections may be limited to certain network namespaces (Linux only), making it easy to build a cross-container,multi-tenant load balancer;

  - PROXY protocol presents the client's IP address to the server even for non-HTTP traffic. This is an HAProxy extension that was adopted by a number of third-party products by now, at least these ones at the time of writing :
      - client : haproxy, stud, stunnel, exaproxy, ELB, squid
      - server : haproxy, stud, postfix, exim, nginx, squid, node.js, varnish
      
      
代理是通过两个独立的连接在客户机和服务器之间传输数据的操作。在代理和连接管理方面，HAProxy支持以下基本特性:

-为服务器提供一个干净的连接，以保护它们免受任何客户端缺陷或攻击;

-监听多个IP地址和/或端口，甚至端口范围;

-透明接受:拦截流量针对任何任意IP地址，甚至不属于本地系统;

服务器端端口不需要与监听端口相关，甚至可以通过固定偏移量进行转换(适用于范围)

-透明连接:欺骗客户端(或任何)的IP地址，如果需要连接到服务器;

-为多站点LBs的服务器提供可靠的返回IP地址;

-卸载服务器感谢缓冲区和可能的短时间连接，以减少并发连接数和内存占用;

-优化TCP栈(如SACK)，拥塞控制，减少RTT影响;

-支持双方不同的协议族(如IPv4/IPv6/Unix);

-超时强制:HAProxy根据连接的阶段支持多个超时级别，因此死亡的客户端或服务器，或攻击者不能被授予资源太长时间;

-协议验证:检查HTTP, SSL或有效负载，拒绝无效的协议元素，除非指示接受它们;

-政策执行:确保只有允许的内容才会被转发;

-传入和传出连接可能仅限于某些网络名称空间(仅限Linux)，使其易于构建跨容器，多租户负载均衡器;

—代理协议将客户端的IP地址呈现给服务器，即使是非http流量。这是一个HAProxy扩展，目前已被许多第三方产品采用，至少在撰写本文时是这样的:
-客户端:haproxy, stud, stunnel, exaproxy, ELB, squid
-服务器:haproxy, stud, postfix, exim, nginx, squid, node.js, varnish
 
```



### [3.3.2.](http://docs.haproxy.org/2.7/intro.html#3.3.2) Basic features : SSL

```
HAProxy's SSL stack is recognized as one of the most featureful according to Google's engineers (http://istlsfastyet.com/). The most commonly used features making it quite complete are :

  - SNI-based multi-hosting with no limit on sites count and focus on performance. At least one deployment is known for running 50000 domains with their respective certificates;

  - support for wildcard certificates reduces the need for many certificates ;

  - certificate-based client authentication with configurable policies on  failure to present a valid certificate. This allows to present a different server farm to regenerate the client certificate for example;

  - authentication of the backend server ensures the backend server is the real  one and not a man in the middle;

  - authentication with the backend server lets the backend server know it's really the expected haproxy node that is connecting to it;

  - TLS NPN and ALPN extensions make it possible to reliably offload SPDY/HTTP2 connections and pass them in clear text to backend servers;

  - OCSP stapling further reduces first page load time by delivering inline an OCSP response when the client requests a Certificate Status Request;

  - Dynamic record sizing provides both high performance and low latency, and significantly reduces page load time by letting the browser start to fetch new objects while packets are still in flight;

  - permanent access to all relevant SSL/TLS layer information for logging, access control, reporting etc. These elements can be embedded into HTTP  header or even as a PROXY protocol extension so that the offloaded server  gets all the information it would have had if it performed the SSL termination itself.

  - Detect, log and block certain known attacks even on vulnerable SSL libs,such as the Heartbleed attack affecting certain versions of OpenSSL.

  - support for stateless session resumption (RFC 5077 TLS Ticket extension).TLS tickets can be updated from CLI which provides them means to implement Perfect Forward Secrecy by frequently rotating the tickets.
  
  
 根据谷歌工程师的说法，HAProxy的SSL堆栈被认为是最有功能的堆栈之一(http://istlsfastyet.com/)。使其相当完整的最常用功能是:

-基于sni的多主机，没有站点数量限制，专注于性能。已知至少有一个部署运行了50000个带有各自证书的域;

对通配符证书的支持减少了对许多证书的需求;

—基于证书的客户端认证，提供可配置的证书失效策略。例如，这允许提供不同的服务器场来重新生成客户端证书;

后端服务器的身份验证确保后端服务器是真实的，而不是一个中间人

-后端服务器的身份验证让后端服务器知道它真的是预期的haproxy节点连接到它;

- TLS NPN和ALPN扩展可以可靠地卸载SPDY/HTTP2连接，并将它们以明文传递到后端服务器;

-当客户端请求证书状态请求时，OCSP钉接通过内联的OCSP响应进一步减少了第一页加载时间;

-动态记录大小提供高性能和低延迟，并通过让浏览器在数据包仍在飞行时开始获取新对象显着减少页面加载时间;

-永久访问所有相关的SSL/TLS层信息，用于日志记录，访问控制，报告等。这些元素可以嵌入到HTTP报头中，甚至可以作为代理协议扩展，以便卸载的服务器获得如果它自己执行SSL终止将拥有的所有信息。

-检测，记录和阻止某些已知的攻击，甚至对易受攻击的SSL库，如心脏出血攻击影响某些版本的OpenSSL。

-支持无状态会话恢复(RFC 5077 TLS票证扩展)。TLS票证可以从CLI更新，这为他们提供了通过频繁轮换票证来实现完美前向保密的方法。
 
```



### [3.3.3.](http://docs.haproxy.org/2.7/intro.html#3.3.3) Basic features : Monitoring

```
HAProxy focuses a lot on availability. As such it cares about servers state,and about reporting its own state to other network components :

  - Servers' state is continuously monitored using per-server parameters. This ensures the path to the server is operational for regular traffic;

  - Health checks support two hysteresis for up and down transitions in order to protect against state flapping;

  - Checks can be sent to a different address/port/protocol : this makes it easy to check a single service that is considered representative of multiple ones, for example the HTTPS port for an HTTP+HTTPS server.

  - Servers can track other servers and go down simultaneously : this ensures that servers hosting multiple services can fail atomically and that no one will be sent to a partially failed server;

  - Agents may be deployed on the server to monitor load and health : a server may be interested in reporting its load, operational status, administrative status independently from what health checks can see. By running a simple agent on the server, it's possible to consider the server's view of its own health in addition to the health checks validating the whole path;

  - Various check methods are available : TCP connect, HTTP request, SMTP hello,SSL hello, LDAP, SQL, Redis, send/expect scripts, all with/without SSL;

  - State change is notified in the logs and stats page with the failure reason(e.g. the HTTP response received at the moment the failure was detected). An e-mail can also be sent to a configurable address upon such a change ;

  - Server state is also reported on the stats interface and can be used to take routing decisions so that traffic may be sent to different farms depending on their sizes and/or health (e.g. loss of an inter-DC link);

  - HAProxy can use health check requests to pass information to the servers,such as their names, weight, the number of other servers in the farm etc.so that servers can adjust their response and decisions based on this knowledge (e.g. postpone backups to keep more CPU available);

  - Servers can use health checks to report more detailed state than just on/off(e.g. I would like to stop, please stop sending new visitors);

  - HAProxy itself can report its state to external components such as routers or other load balancers, allowing to build very complete multi-path and multi-layer infrastructures.
  
  
HAProxy非常关注可用性。因此，它关心服务器状态，并将自己的状态报告给其他网络组件:

—通过每台服务器的参数持续监控服务器的状态。这确保了通往服务器的路径对于常规流量是可操作的;

-健康检查支持上下转换的两个滞后，以防止状态振荡;

-检查可以发送到不同的地址/端口/协议:这使得它很容易检查一个单一的服务，被认为是多个的代表，例如HTTP+HTTPS服务器的HTTPS端口。

-服务器可以跟踪其他服务器并同时关闭:这确保了托管多个服务的服务器可以自动失败，并且没有人会被发送到部分失败的服务器;

—可以在服务器上部署代理以监视负载和运行状况:服务器可能对报告其负载、操作状态、管理状态感兴趣，而不依赖于运行状况检查可以看到的内容。通过在服务器上运行一个简单的代理，除了验证整个路径的运行状况检查外，还可以考虑服务器自身运行状况的视图;

-各种检查方法可用:TCP连接，HTTP请求，SMTP hello,SSL hello, LDAP, SQL, Redis，发送/期望脚本，所有有/没有SSL;

-状态更改在日志和状态页面中通知失败原因(例如:在检测到故障时收到的HTTP响应)。电子邮件也可以发送到一个可配置的地址在这样的变化;

服务器状态也会在统计界面上报告，并且可以用来做出路由决定，这样流量就可以根据它们的大小和/或健康状况被发送到不同的农场(例如，dc间链路的丢失)

- HAProxy可以使用健康检查请求将信息传递给服务器，例如它们的名称，权重，场中其他服务器的数量等，以便服务器可以根据这些知识调整其响应和决策(例如推迟备份以保持更多CPU可用);

服务器可以使用健康检查来报告比开/关更详细的状态(例如:我想停止，请停止发送新的访客);

- HAProxy本身可以向外部组件(如路由器或其他负载平衡器)报告其状态，允许构建非常完整的多路径和多层基础设施。
 
```



### [3.3.4.](http://docs.haproxy.org/2.7/intro.html#3.3.4) Basic features : High availability

```
Just like any serious load balancer, HAProxy cares a lot about availability to ensure the best global service continuity :

  - Only valid servers are used ; the other ones are automatically evicted from load balancing farms ; under certain conditions it is still possible to force to use them though;

  - Support for a graceful shutdown so that it is possible to take servers out of a farm without affecting any connection;

  - Backup servers are automatically used when active servers are down and replace them so that sessions are not lost when possible. This also allows to build multiple paths to reach the same server (e.g. multiple interfaces);

  - Ability to return a global failed status for a farm when too many servers are down. This, combined with the monitoring capabilities makes it possible for an upstream component to choose a different LB node for a given service;

  - Stateless design makes it easy to build clusters : by design, HAProxy does its best to ensure the highest service continuity without having to store information that could be lost in the event of a failure. This ensures that a takeover is the most seamless possible;

  - Integrates well with standard VRRP daemon keepalived : HAProxy easily tells keepalived about its state and copes very well with floating virtual IP addresses. Note: only use IP redundancy protocols (VRRP/CARP) over cluster-based solutions (Heartbeat, ...) as they're the ones offering the fastest,most seamless, and most reliable switchover.
  
  就像任何严肃的负载平衡器一样，HAProxy非常关心可用性，以确保最佳的全球服务连续性:

-只使用有效的服务器;其他的被自动从负载均衡场中驱逐;但在某些情况下，仍有可能强制使用它们;

-支持一个优雅的关闭，这样就可以在不影响任何连接的情况下将服务器带出一个场;

—当主服务器宕机时，备份服务器会自动使用并替换备用服务器，尽可能避免会话丢失。这也允许建立多个路径到达同一个服务器(例如多个接口);

-当太多服务器关闭时，能够返回一个农场的全局失败状态。结合监控功能，上游组件可以为给定的服务选择不同的LB节点;

-无状态设计使构建集群变得容易:通过设计，HAProxy尽其所能确保最高的服务连续性，而不必存储在发生故障时可能丢失的信息。这确保了收购是最无缝的;

-与标准VRRP守护进程keepalived很好地集成:HAProxy可以很容易地告诉keepalived它的状态，并且可以很好地处理浮动虚拟IP地址。注意:只使用IP冗余协议(VRRP/CARP)而不是基于集群的解决方案(Heartbeat，…)，因为它们提供最快、最无缝和最可靠的切换。
 
```



### [3.3.5.](http://docs.haproxy.org/2.7/intro.html#3.3.5) Basic features : Load balancing

```
HAProxy offers a fairly complete set of load balancing features, most of which are unfortunately not available in a number of other load balancing products :

  - no less than 10 load balancing algorithms are supported, some of which apply to input data to offer an infinite list of possibilities. The most common ones are round-robin (for short connections, pick each server in turn),leastconn (for long connections, pick the least recently used of the servers with the lowest connection count), source (for SSL farms or terminal server farms, the server directly depends on the client's source address), URI (for HTTP caches, the server directly depends on the HTTP URI), hdr (the server directly depends on the contents of a specific HTTP header field), first(for short-lived virtual machines, all connections are packed on the smallest possible subset of servers so that unused ones can be powered down);

  - all algorithms above support per-server weights so that it is possible to accommodate from different server generations in a farm, or direct a small fraction of the traffic to specific servers (debug mode, running the next version of the software, etc);

  - dynamic weights are supported for round-robin, leastconn and consistent hashing ; this allows server weights to be modified on the fly from the CLI or even by an agent running on the server;

  - slow-start is supported whenever a dynamic weight is supported; this allows a server to progressively take the traffic. This is an important feature for fragile application servers which require to compile classes at runtime as well as cold caches which need to fill up before being run at full throttle;

  - hashing can apply to various elements such as client's source address, URL components, query string element, header field values, POST parameter, RDP cookie;

  - consistent hashing protects server farms against massive redistribution when adding or removing servers in a farm. That's very important in large cache farms and it allows slow-start to be used to refill cold caches;

  - a number of internal metrics such as the number of connections per server,per backend, the amount of available connection slots in a backend etc makes it possible to build very advanced load balancing strategies.
  
  
  
HAProxy提供了一套相当完整的负载均衡功能，不幸的是，其中大多数功能在许多其他负载均衡产品中是不可用的:

-支持不少于10种负载平衡算法，其中一些算法适用于输入数据，以提供无限的可能性列表。最常见的是轮询(对于短连接，依次选择每个服务器)，最小连接(对于长连接，选择最近最少使用的连接数最少的服务器)，源(对于SSL农场或终端服务器农场，服务器直接依赖于客户端的源地址)，URI(对于HTTP缓存，服务器直接依赖于HTTP URI)， hdr(服务器直接依赖于特定HTTP头字段的内容)，第一(对于短暂的虚拟机，所有的连接都被打包在尽可能小的服务器子集上，这样不使用的连接就可以关闭);

-以上所有算法都支持每个服务器的权重，这样就可以在一个农场中容纳来自不同服务器代的流量，或者将一小部分流量引导到特定的服务器(调试模式，运行下一个版本的软件等)

-支持动态权重轮询，最小连接和一致哈希;这允许服务器权重可以从CLI动态修改，甚至可以由运行在服务器上的代理修改;

-当支持动态权重时，支持慢启动;这允许服务器逐步接收流量。对于需要在运行时编译类的脆弱应用服务器以及需要在全速运行之前填满的冷缓存来说，这是一个重要的特性;

-哈希可以应用于各种元素，如客户端的源地址，URL组件，查询字符串元素，报头字段值，POST参数，RDP cookie;

-一致性哈希保护服务器场在增加或删除服务器时免受大规模重新分配。这在大型缓存场中非常重要，它允许慢启动来重新填充冷缓存;

-一些内部指标，如每台服务器的连接数，每个后端，后端可用连接槽的数量等，使构建非常先进的负载平衡策略成为可能。
 
```



### [3.3.6.](http://docs.haproxy.org/2.7/intro.html#3.3.6) Basic features : Stickiness

```
Application load balancing would be useless without stickiness. HAProxy provides a fairly comprehensive set of possibilities to maintain a visitor on the same server even across various events such as server addition/removal, down/up cycles, and some methods are designed to be resistant to the distance between multiple load balancing nodes in that they don't require any replication :

  - stickiness information can be individually matched and learned from different places if desired. For example a JSESSIONID cookie may be matched both in a cookie and in the URL. Up to 8 parallel sources can be learned at the same time and each of them may point to a different stick-table;

  - stickiness information can come from anything that can be seen within a request or response, including source address, TCP payload offset and length, HTTP query string elements, header field values, cookies, and so on.

  - stick-tables are replicated between all nodes in a multi-master fashion;

  - commonly used elements such as SSL-ID or RDP cookies (for TSE farms) are directly accessible to ease manipulation;

  - all sticking rules may be dynamically conditioned by ACLs;

  - it is possible to decide not to stick to certain servers, such as backup servers, so that when the nominal server comes back, it automatically takes the load back. This is often used in multi-path environments;

  - in HTTP it is often preferred not to learn anything and instead manipulate a cookie dedicated to stickiness. For this, it's possible to detect, rewrite, insert or prefix such a cookie to let the client remember what server was assigned;

  - the server may decide to change or clean the stickiness cookie on logout,so that leaving visitors are automatically unbound from the server;

  - using ACL-based rules it is also possible to selectively ignore or enforce stickiness regardless of the server's state; combined with advanced health checks, that helps admins verify that the server they're installing is up and running before presenting it to the whole world;

  - an innovative mechanism to set a maximum idle time and duration on cookies ensures that stickiness can be smoothly stopped on devices which are never closed (smartphones, TVs, home appliances) without having to store them on persistent storage;

  - multiple server entries may share the same stickiness keys so that stickiness is not lost in multi-path environments when one path goes down;

  - soft-stop ensures that only users with stickiness information will continue to reach the server they've been assigned to but no new users will go there.
  
  
  如果没有粘性，应用程序负载平衡将是无用的。HAProxy提供了一组相当全面的可能性，即使在服务器添加/删除，上下循环等各种事件中，也可以在同一服务器上维护访问者，并且一些方法被设计为能够抵抗多个负载平衡节点之间的距离，因为它们不需要任何复制:

-粘性信息可以单独匹配，如果需要，可以从不同的地方学习。例如，JSESSIONID cookie可能在cookie和URL中都匹配。最多可以同时学习8个平行源，每个源可以指向不同的棒表;

-粘性信息可以来自任何可以在请求或响应中看到的内容，包括源地址、TCP有效载荷偏移量和长度、HTTP查询字符串元素、报头字段值、cookie等。

-粘贴表在所有节点之间以多主方式复制;

-常用的元素，如SSL-ID或RDP cookie(用于TSE农场)可以直接访问，以简化操作;

-所有黏附规则可由acl动态调节;

-可以决定不固定在某些服务器上，比如备份服务器，这样当名义服务器回来时，它会自动承担负载。这通常用于多路径环境;

-在HTTP中，通常不希望学习任何东西，而是操作专门用于粘性的cookie。为此，可以检测，重写，插入或前缀这样的cookie，让客户端记住服务器被分配;

-伺服器可能决定在登出时更改或清除黏性cookie，以便离开的访客自动与伺服器解除绑定;

使用基于acl的规则，也可以选择性地忽略或强制粘性，而不管服务器的状态结合先进的健康检查，这有助于管理员在向全世界展示他们正在安装的服务器之前验证它是启动和运行的;

-一个创新的机制来设置cookie的最大空闲时间和持续时间，确保粘性可以在从未关闭的设备(智能手机，电视，家用电器)上顺利停止，而不必将它们存储在持久存储上;

-多个服务器条目可以共享相同的粘性密钥，这样当一个路径出现故障时，粘性不会在多路径环境中丢失

软停止确保只有带有粘性信息的用户才会继续访问他们已经分配到的服务器，而没有新用户会去那里。
 
```



### [3.3.7.](http://docs.haproxy.org/2.7/intro.html#3.3.7) Basic features : Logging

```
Logging is an extremely important feature for a load balancer, first because a load balancer is often wrongly accused of causing the problems it reveals, and second because it is placed at a critical point in an infrastructure where all normal and abnormal activity needs to be analyzed and correlated with other components.

HAProxy provides very detailed logs, with millisecond accuracy and the exact connection accept time that can be searched in firewalls logs (e.g. for NAT correlation). By default, TCP and HTTP logs are quite detailed and contain everything needed for troubleshooting, such as source IP address and port,frontend, backend, server, timers (request receipt duration, queue duration,connection setup time, response headers time, data transfer time), global process state, connection counts, queue status, retries count, detailed stickiness actions and disconnect reasons, header captures with a safe output encoding. It is then possible to extend or replace this format to include any sampled data, variables, captures, resulting in very detailed information. For example it is possible to log the number of cumulative requests or number of different URLs visited by a client.

The log level may be adjusted per request using standard ACLs, so it is possible to automatically silent some logs considered as pollution and instead raise warnings when some abnormal behavior happen for a small part of the traffic (e.g. too many URLs or HTTP errors for a source address). Administrative logs are also emitted with their own levels to inform about the loss or recovery of a server for example.

Each frontend and backend may use multiple independent log outputs, which eases multi-tenancy. Logs are preferably sent over UDP, maybe JSON-encoded, and are truncated after a configurable line length in order to guarantee delivery. But it is also possible to send them to stdout/stderr or any file descriptor, as well as to a ring buffer that a client can subscribe to in order to retrieve them.


日志记录是负载平衡器的一个极其重要的特性，首先因为负载平衡器经常被错误地指责为导致它所揭示的问题的原因，其次因为它被置于基础设施的关键点上，需要分析所有正常和异常的活动，并将其与其他组件关联起来。

HAProxy提供了非常详细的日志，具有毫秒级的精度和可以在防火墙日志中搜索的确切连接接受时间(例如NAT相关性)。默认情况下，TCP和HTTP日志非常详细，包含故障排除所需的所有内容，例如源IP地址和端口，前端，后端，服务器，计时器(请求接收持续时间，队列持续时间，连接建立时间，响应标头时间，数据传输时间)，全局进程状态，连接计数，队列状态，重试计数，详细的粘性动作和断开原因，标头捕获与安全输出编码。然后可以扩展或替换此格式，以包含任何采样数据、变量、捕获，从而产生非常详细的信息。例如，可以记录累积请求的数量或客户端访问的不同url的数量。

日志级别可以根据每个请求使用标准acl进行调整，因此可以自动关闭一些被认为是污染的日志，而不是在一小部分流量发生一些异常行为时提出警告(例如，源地址的url或HTTP错误太多)。管理日志也会以自己的级别发出，例如通知服务器的丢失或恢复。

每个前端和后端可以使用多个独立的日志输出，从而简化了多租户。日志最好通过UDP发送，可能是json编码的，并且在可配置的行长度之后被截断，以保证传输。但是也可以将它们发送到stdout/stderr或任何文件描述符，以及发送到客户端可以订阅的循环缓冲区以检索它们。
 
```



### [3.3.8.](http://docs.haproxy.org/2.7/intro.html#3.3.8) Basic features : Statistics

```
HAProxy provides a web-based statistics reporting interface with authentication,security levels and scopes. It is thus possible to provide each hosted customer with his own page showing only his own instances. This page can be located in a hidden URL part of the regular web site so that no new port needs to be opened.This page may also report the availability of other HAProxy nodes so that it is easy to spot if everything works as expected at a glance. The view is synthetic with a lot of details accessible (such as error causes, last access and last change duration, etc), which are also accessible as a CSV table that other tools may import to draw graphs. The page may self-refresh to be used as a monitoring page on a large display. In administration mode, the page also allows to change server state to ease maintenance operations.

A Prometheus exporter is also provided so that the statistics can be consumed in a different format depending on the deployment.


HAProxy提供了一个基于web的统计报告接口，包括身份验证、安全级别和范围。因此，可以为每个托管客户提供他自己的页面，仅显示他自己的实例。此页面可以位于常规网站的隐藏URL部分，因此不需要打开新端口。该页还可以报告其他HAProxy节点的可用性，以便一目了然地发现一切是否如预期的那样工作。视图是合成的，有许多可访问的细节(例如错误原因、最后一次访问和最后一次更改持续时间等)，也可以作为CSV表访问，其他工具可以导入该表来绘制图形。该页可以自我刷新，用作大型显示器上的监视页。在管理模式下，该页面还允许更改服务器状态，以简化维护操作。

还提供了Prometheus导出器，以便可以根据部署以不同的格式使用统计数据。
 
```



## [3.4.](http://docs.haproxy.org/2.7/intro.html#3.4) Standard features

```
In this section, some features that are very commonly used in HAProxy but are not necessarily present on other load balancers are enumerated.

在本节中，列举了一些在HAProxy中非常常用但不一定出现在其他负载平衡器上的特性。
```



### [3.4.1.](http://docs.haproxy.org/2.7/intro.html#3.4.1) Standard features : Sampling and converting information

```
HAProxy supports information sampling using a wide set of "sample fetch functions". The principle is to extract pieces of information known as samples,for immediate use. This is used for stickiness, to build conditions, to produce information in logs or to enrich HTTP headers.

Samples can be fetched from various sources :

  - constants : integers, strings, IP addresses, binary blocks;

  - the process : date, environment variables, server/frontend/backend/process state, byte/connection counts/rates, queue length, random generator, ...

  - variables : per-session, per-request, per-response variables;

  - the client connection : source and destination addresses and ports, and all related statistics counters;

  - the SSL client session : protocol, version, algorithm, cipher, key size,session ID, all client and server certificate fields, certificate serial,SNI, ALPN, NPN, client support for certain extensions;

  - request and response buffers contents : arbitrary payload at offset/length,data length, RDP cookie, decoding of SSL hello type, decoding of TLS SNI;

  - HTTP (request and response) : method, URI, path, query string arguments,status code, headers values, positional header value, cookies, captures,authentication, body elements;

A sample may then pass through a number of operators known as "converters" to experience some transformation. A converter consumes a sample and produces a new one, possibly of a completely different type. For example, a converter may be used to return only the integer length of the input string, or could turn a string to upper case. Any arbitrary number of converters may be applied in series to a sample before final use. Among all available sample converters, the following ones are the most commonly used :

  - arithmetic and logic operators : they make it possible to perform advanced computation on input data, such as computing ratios, percentages or simply converting from one unit to another one;

  - IP address masks are useful when some addresses need to be grouped by larger networks;

  - data representation : URL-decode, base64, hex, JSON strings, hashing;

  - string conversion : extract substrings at fixed positions, fixed length,extract specific fields around certain delimiters, extract certain words,change case, apply regex-based substitution;

  - date conversion : convert to HTTP date format, convert local to UTC and conversely, add or remove offset;

  - lookup an entry in a stick table to find statistics or assigned server;

  - map-based key-to-value conversion from a file (mostly used for geolocation).
  
  
  
  HAProxy使用一系列“样本获取函数”来支持信息采样。其原理是提取被称为样本的信息片段，以便立即使用。它用于增强粘性、构建条件、在日志中生成信息或丰富HTTP标头。

样本可以从不同的来源获取:

-常量:整数，字符串，IP地址，二进制块;

-进程:日期，环境变量，服务器/前端/后端/进程状态，字节/连接计数/速率，队列长度，随机生成器，…

-变量:每个会话，每个请求，每个响应变量;

-客户端连接:源和目的地址和端口，以及所有相关的统计计数器;

- SSL客户端会话:协议，版本，算法，密码，密钥大小，会话ID，所有客户端和服务器证书字段，证书串行，SNI, ALPN, NPN，客户端支持某些扩展;

-请求和响应缓冲区内容:任意有效载荷在偏移/长度，数据长度，RDP cookie，解码SSL hello类型，解码TLS SNI;

- HTTP(请求和响应):方法、URI、路径、查询字符串参数、状态码、报头值、位置报头值、cookie、捕获、认证、正文元素;

然后，样品可能会经过一些称为“转换器”的操作员来经历一些转换。转换器消耗一个样本并产生一个新的样本，可能是一个完全不同的类型。例如，转换器可用于仅返回输入字符串的整数长度，或将字符串转换为大写。在最终使用前，可以将任意数量的转换器串联到样品上。在所有可用的示例转换器中，以下是最常用的转换器:

-算术和逻辑运算符:它们使对输入数据进行高级计算成为可能，例如计算比率、百分比或简单地从一个单位转换到另一个单位;

- IP地址掩码是有用的，当一些地址需要由较大的网络分组;

-数据表示:url解码，base64，十六进制，JSON字符串，哈希;

-字符串转换:提取固定位置的子字符串，固定长度，提取特定字段周围的某些分隔符，提取某些单词，更改大小写，应用基于正则表达式的替代;

-日期转换:转换到HTTP日期格式，转换本地到UTC和相反，添加或删除偏移量;

-查找一个条目在棒表查找统计数据或分配的服务器;

-基于映射的从文件的键到值转换(主要用于地理定位)。
 
```



### [3.4.2.](http://docs.haproxy.org/2.7/intro.html#3.4.2) Standard features : Maps

```
Maps are a powerful type of converter consisting in loading a two-columns file into memory at boot time, then looking up each input sample from the first column and either returning the corresponding pattern on the second column if the entry was found, or returning a default value. The output information also being a sample, it can in turn experience other transformations including other map lookups. Maps are most commonly used to translate the client's IP address to an AS number or country code since they support a longest match for network addresses but they can be used for various other purposes.

Part of their strength comes from being updatable on the fly either from the CLI or from certain actions using other samples, making them capable of storing and retrieving information between subsequent accesses. Another strength comes from the binary tree based indexation which makes them extremely fast even when they contain hundreds of thousands of entries, making geolocation very cheap and easy to set up.


映射是一种功能强大的转换器，它在引导时将两列文件加载到内存中，然后从第一列查找每个输入示例，如果找到条目，则在第二列返回相应的模式，或者返回默认值。输出信息也是一个示例，它可以依次经历其他转换，包括其他映射查找。映射最常用于将客户端的IP地址转换为AS号或国家代码，因为它们支持网络地址的最长匹配，但它们可以用于各种其他目的。

它们的部分优势在于可以从CLI或使用其他示例的某些操作中动态更新，从而使它们能够在后续访问之间存储和检索信息。另一个优势来自于基于二叉树的索引，这使得它们非常快，即使它们包含数十万个条目，也使得地理定位非常便宜且易于设置。
 
```



### [3.4.3.](http://docs.haproxy.org/2.7/intro.html#3.4.3) Standard features : ACLs and conditions

```
Most operations in HAProxy can be made conditional. Conditions are built by combining multiple ACLs using logic operators (AND, OR, NOT). Each ACL is a series of tests based on the following elements :

  - a sample fetch method to retrieve the element to test ;

  - an optional series of converters to transform the element ;

  - a list of patterns to match against ;

  - a matching method to indicate how to compare the patterns with the sample

For example, the sample may be taken from the HTTP "Host" header, it could then be converted to lower case, then matched against a number of regex patterns using the regex matching method.

Technically, ACLs are built on the same core as the maps, they share the exact same internal structure, pattern matching methods and performance. The only real difference is that instead of returning a sample, they only return "found" or "not found". In terms of usage, ACL patterns may be declared inline in the configuration file and do not require their own file. ACLs may be named for ease of use or to make configurations understandable. A named ACL may be declared multiple times and it will evaluate all definitions in turn until one matches.

About 13 different pattern matching methods are provided, among which IP address mask, integer ranges, substrings, regex. They work like functions, and just like with any programming language, only what is needed is evaluated, so when a condition involving an OR is already true, next ones are not evaluated, and similarly when a condition involving an AND is already false, the rest of the condition is not evaluated.

There is no practical limit to the number of declared ACLs, and a handful of commonly used ones are provided. However experience has shown that setups using a lot of named ACLs are quite hard to troubleshoot and that sometimes using anonymous ACLs inline is easier as it requires less references out of the scope being analyzed.


HAProxy中的大多数操作都是有条件的。通过使用逻辑运算符(AND、OR、NOT)组合多个acl来构建条件。每个ACL是基于以下元素的一系列测试:

-取取要测试元素的样本方法;

-可选的一系列转换器来转换元素;

-要匹配的模式列表;

-一种匹配方法，用于指示如何将图案与样品进行比较

例如，样本可能取自HTTP“Host”报头，然后将其转换为小写，然后使用regex匹配方法与许多regex模式进行匹配。

从技术上讲，acl与映射构建在相同的核心上，它们共享完全相同的内部结构、模式匹配方法和性能。唯一真正的区别是，它们只返回“找到”或“未找到”，而不是返回样本。在使用方面，ACL模式可以在配置文件中内联声明，不需要它们自己的文件。acl的命名可能是为了便于使用或使配置易于理解。命名ACL可以声明多次，它将依次评估所有定义，直到匹配为止。

提供了大约13种不同的模式匹配方法，其中包括IP地址掩码、整数范围、子字符串、正则表达式。它们像函数一样工作，就像任何编程语言一样，只计算需要的部分，因此，当涉及OR的条件已经为真时，不会计算下一个条件，类似地，当涉及and的条件已经为假时，不会计算条件的其余部分。

对声明的acl数量没有实际限制，并提供了一些常用的acl。然而，经验表明，使用大量命名acl的设置很难排除故障，有时内联使用匿名acl更容易，因为它需要较少的超出所分析范围的引用。
 
```



### [3.4.4.](http://docs.haproxy.org/2.7/intro.html#3.4.4) Standard features : Content switching

```
HAProxy implements a mechanism known as content-based switching. The principle is that a connection or request arrives on a frontend, then the information carried with this request or connection are processed, and at this point it is possible to write ACLs-based conditions making use of these information to decide what backend will process the request. Thus the traffic is directed to one backend or another based on the request's contents. The most common example consists in using the Host header and/or elements from the path (sub-directories or file-name extensions) to decide whether an HTTP request targets a static object or the application, and to route static objects traffic to a backend made of fast and light servers, and all the remaining traffic to a more complex application server, thus constituting a fine-grained virtual hosting solution.
This is quite convenient to make multiple technologies coexist as a more global solution.

Another use case of content-switching consists in using different load balancing algorithms depending on various criteria. A cache may use a URI hash while an application would use round-robin.

Last but not least, it allows multiple customers to use a small share of a common resource by enforcing per-backend (thus per-customer connection limits).

Content switching rules scale very well, though their performance may depend on the number and complexity of the ACLs in use. But it is also possible to write dynamic content switching rules where a sample value directly turns into a backend name and without making use of ACLs at all. Such configurations have been reported to work fine at least with 300000 backends in production.

HAProxy实现了一种称为基于内容的交换的机制。其原理是，连接或请求到达前端，然后处理该请求或连接所携带的信息，此时可以编写基于acl的条件，利用这些信息来决定哪个后端将处理请求。因此，流量根据请求的内容定向到一个后端或另一个后端。最常见的示例包括使用Host头和/或路径中的元素(子目录或文件名扩展名)来确定HTTP请求是针对静态对象还是应用程序，并将静态对象流量路由到由快速和轻量级服务器组成的后端，并将所有剩余流量路由到更复杂的应用程序服务器，从而构成细粒度的虚拟主机解决方案。
这使得多种技术作为一个更全局的解决方案共存非常方便。

内容交换的另一个用例是根据不同的标准使用不同的负载平衡算法。缓存可能使用URI哈希，而应用程序可能使用轮询。

最后但并非最不重要的一点是，它允许多个客户通过强制每个后端(因此每个客户连接限制)使用公共资源的一小部分。

内容切换规则的伸缩性非常好，尽管它们的性能可能取决于所使用的acl的数量和复杂性。但是也可以编写动态内容切换规则，其中示例值直接转换为后端名称，而根本不使用acl。据报道，这种配置至少在生产中有300000个后端时工作良好。
 
```



### [3.4.5.](http://docs.haproxy.org/2.7/intro.html#3.4.5) Standard features : Stick-tables

```
Stick-tables are commonly used to store stickiness information, that is, to keep a reference to the server a certain visitor was directed to. The key is then the identifier associated with the visitor (its source address, the SSL ID of the connection, an HTTP or RDP cookie, the customer number extracted from the URL or from the payload, ...) and the stored value is then the server's identifier.

Stick tables may use 3 different types of samples for their keys : integers,strings and addresses. Only one stick-table may be referenced in a proxy, and it is designated everywhere with the proxy name. Up to 8 keys may be tracked in parallel. The server identifier is committed during request or response processing once both the key and the server are known.

Stick-table contents may be replicated in active-active mode with other HAProxy nodes known as "peers" as well as with the new process during a reload operation so that all load balancing nodes share the same information and take the same routing decision if client's requests are spread over multiple nodes.

Since stick-tables are indexed on what allows to recognize a client, they are often also used to store extra information such as per-client statistics. The extra statistics take some extra space and need to be explicitly declared. The type of statistics that may be stored includes the input and output bandwidth,the number of concurrent connections, the connection rate and count over a period, the amount and frequency of errors, some specific tags and counters,etc. In order to support keeping such information without being forced to stick to a given server, a special "tracking" feature is implemented and allows to track up to 3 simultaneous keys from different tables at the same time regardless of stickiness rules. Each stored statistics may be searched, dumped and cleared from the CLI and adds to the live troubleshooting capabilities.

While this mechanism can be used to surclass a returning visitor or to adjust the delivered quality of service depending on good or bad behavior, it is mostly used to fight against service abuse and more generally DDoS as it allows to build complex models to detect certain bad behaviors at a high processing speed.


粘性表通常用于存储粘性信息，即保留对某个访问者被定向到的服务器的引用。然后，键是与访问者相关联的标识符(它的源地址、连接的SSL ID、HTTP或RDP cookie、从URL或有效负载中提取的客户编号等)，存储的值是服务器的标识符。

Stick表可以使用3种不同类型的样本作为键:整数、字符串和地址。在一个代理中只能引用一个棒表，并且它在任何地方都用代理名称指定。最多可并行跟踪8个键。一旦密钥和服务器都已知，就在请求或响应处理期间提交服务器标识符。

粘贴表内容可以在双活模式下与其他HAProxy节点(称为“对等体”)以及在重新加载操作期间与新进程进行复制，以便所有负载平衡节点共享相同的信息，并在客户端请求分布在多个节点时采取相同的路由决策。

由于粘接表是根据允许识别客户机的内容进行索引的，因此它们通常还用于存储额外的信息，例如每个客户机的统计信息。额外的统计信息占用一些额外的空间，需要显式声明。可以存储的统计类型包括输入和输出带宽、并发连接数、一段时间内的连接速率和计数、错误的数量和频率、一些特定的标签和计数器等。为了支持保存这些信息，而不必被迫固定在给定的服务器上，实现了一个特殊的“跟踪”功能，允许同时跟踪来自不同表的最多3个键，而不考虑粘性规则。可以在CLI中搜索、转储和清除每个存储的统计信息，并添加实时故障排除功能。

虽然此机制可用于超越回访者或根据好或坏的行为调整交付的服务质量，但它主要用于对抗服务滥用和更常见的DDoS，因为它允许构建复杂的模型以高处理速度检测某些不良行为。
```



### [3.4.6.](http://docs.haproxy.org/2.7/intro.html#3.4.6) Standard features : Formatted strings

```
There are many places where HAProxy needs to manipulate character strings, such as logs, redirects, header additions, and so on. In order to provide the greatest flexibility, the notion of Formatted strings was introduced, initially for logging purposes, which explains why it's still called "log-format". These strings contain escape characters allowing to introduce various dynamic data including variables and sample fetch expressions into strings, and even to adjust the encoding while the result is being turned into a string (for example,adding quotes). This provides a powerful way to build header contents, to build response data or even response templates, or to customize log lines.
Additionally, in order to remain simple to build most common strings, about 50 special tags are provided as shortcuts for information commonly used in logs.


HAProxy需要在很多地方操作字符串，比如日志、重定向、添加报头等等。为了提供最大的灵活性，引入了格式化字符串的概念，最初是为了记录目的，这解释了为什么它仍然被称为“日志格式”。这些字符串包含转义字符，允许将各种动态数据(包括变量和示例获取表达式)引入字符串，甚至可以在将结果转换为字符串时调整编码(例如，添加引号)。这提供了一种强大的方式来构建标头内容、构建响应数据甚至响应模板，或自定义日志行。
此外，为了保持构建大多数常见字符串的简单性，提供了大约50个特殊标记作为日志中常用信息的快捷方式。
```



### [3.4.7.](http://docs.haproxy.org/2.7/intro.html#3.4.7) Standard features : HTTP rewriting and redirection

```
Installing a load balancer in front of an application that was never designed for this can be a challenging task without the proper tools. One of the most commonly requested operation in this case is to adjust requests and response headers to make the load balancer appear as the origin server and to fix hard coded information. This comes with changing the path in requests (which is strongly advised against), modifying Host header field, modifying the Location response header field for redirects, modifying the path and domain attribute for cookies, and so on. It also happens that a number of servers are somewhat verbose and tend to leak too much information in the response, making them more vulnerable to targeted attacks. While it's theoretically not the role of a load balancer to clean this up, in practice it's located at the best place in the infrastructure to guarantee that everything is cleaned up.

Similarly, sometimes the load balancer will have to intercept some requests and respond with a redirect to a new target URL. While some people tend to confuse redirects and rewriting, these are two completely different concepts, since the rewriting makes the client and the server see different things (and disagree on the location of the page being visited) while redirects ask the client to visit the new URL so that it sees the same location as the server.

In order to do this, HAProxy supports various possibilities for rewriting and redirects, among which :

  - regex-based URL and header rewriting in requests and responses. Regex are the most commonly used tool to modify header values since they're easy to manipulate and well understood;

  - headers may also be appended, deleted or replaced based on formatted strings so that it is possible to pass information there (e.g. client side TLS algorithm and cipher);

  - HTTP redirects can use any 3xx code to a relative, absolute, or completely dynamic (formatted string) URI;

  - HTTP redirects also support some extra options such as setting or clearing a specific cookie, dropping the query string, appending a slash if missing,and so on;

  - a powerful "return" directive allows to customize every part of a response like status, headers, body using dynamic contents or even template files.

  - all operations support ACL-based conditions;
  
  如果没有适当的工具，在从未为此设计的应用程序前面安装负载平衡器可能是一项具有挑战性的任务。在这种情况下，最常见的请求操作之一是调整请求和响应头，使负载平衡器显示为源服务器，并修复硬编码信息。这包括更改请求中的路径(强烈建议不要这样做)、修改Host报头字段、修改重定向的Location响应报头字段、修改cookie的路径和域属性等等。还有一种情况是，许多服务器有些冗长，往往会在响应中泄露太多信息，使它们更容易受到有针对性的攻击。虽然从理论上讲，负载平衡器的作用不是清理这些，但在实践中，它位于基础设施中的最佳位置，以确保所有内容都被清理干净。

类似地，有时负载平衡器将不得不拦截一些请求，并响应重定向到新的目标URL。虽然有些人倾向于混淆重定向和重写，但这是两个完全不同的概念，因为重写使客户端和服务器看到不同的东西(并且不同意访问页面的位置)，而重定向要求客户端访问新的URL，以便它看到与服务器相同的位置。

为了做到这一点，HAProxy支持多种重写和重定向的可能性，其中包括:

-在请求和响应中基于regex的URL和header重写。Regex是最常用的修改头值的工具，因为它们易于操作和理解;

-头也可以追加，删除或替换基于格式化字符串，以便有可能传递信息(例如客户端TLS算法和密码);

- HTTP重定向可以使用任何3xx代码到一个相对的，绝对的，或完全动态(格式化字符串)URI;

- HTTP重定向也支持一些额外的选项，如设置或清除特定的cookie，删除查询字符串，如果缺少附加斜杠，等等;

强大的“return”指令允许使用动态内容甚至模板文件自定义响应的每个部分，如状态、头、体。

-所有操作都支持基于acl的条件;
 
```



### [3.4.8.](http://docs.haproxy.org/2.7/intro.html#3.4.8) Standard features : Server protection

```
HAProxy does a lot to maximize service availability, and for this it takes large efforts to protect servers against overloading and attacks. The first and most important point is that only complete and valid requests are forwarded to the servers. The initial reason is that HAProxy needs to find the protocol elements it needs to stay synchronized with the byte stream, and the second reason is that until the request is complete, there is no way to know if some elements will change its semantics. The direct benefit from this is that servers are not exposed to invalid or incomplete requests. This is a very effective protection against slowloris attacks, which have almost no impact on HAProxy.

Another important point is that HAProxy contains buffers to store requests and responses, and that by only sending a request to a server when it's complete and by reading the whole response very quickly from the local network, the server side connection is used for a very short time and this preserves server resources as much as possible.

A direct extension to this is that HAProxy can artificially limit the number of concurrent connections or outstanding requests to a server, which guarantees that the server will never be overloaded even if it continuously runs at 100% of its capacity during traffic spikes. All excess requests will simply be queued to be processed when one slot is released. In the end, this huge resource savings most often ensures so much better server response times that it ends up actually being faster than by overloading the server. Queued requests may be redispatched to other servers, or even aborted in queue when the client aborts, which also protects the servers against the "reload effect", where each click on "reload" by a visitor on a slow-loading page usually induces a new request and maintains the server in an overloaded state.

The slow-start mechanism also protects restarting servers against high traffic levels while they're still finalizing their startup or compiling some classes.

Regarding the protocol-level protection, it is possible to relax the HTTP parser to accept non standard-compliant but harmless requests or responses and even to fix them. This allows bogus applications to be accessible while a fix is being developed. In parallel, offending messages are completely captured with a detailed report that help developers spot the issue in the application. The most dangerous protocol violations are properly detected and dealt with and fixed. For example malformed requests or responses with two Content-length headers are either fixed if the values are exactly the same, or rejected if they differ,since it becomes a security problem. Protocol inspection is not limited to HTTP,it is also available for other protocols like TLS or RDP.

When a protocol violation or attack is detected, there are various options to respond to the user, such as returning the common "HTTP 400 bad request",closing the connection with a TCP reset, or faking an error after a long delay ("tarpit") to confuse the attacker. All of these contribute to protecting the servers by discouraging the offending client from pursuing an attack that becomes very expensive to maintain.

HAProxy also proposes some more advanced options to protect against accidental data leaks and session crossing. Not only it can log suspicious server responsesbut it will also log and optionally block a response which might affect a given visitors' confidentiality. One such example is a cacheable cookie appearing in a cacheable response and which may result in an intermediary cache to deliver it to another visitor, causing an accidental session sharing.


HAProxy在最大化服务可用性方面做了很多工作，为此，它需要付出很大的努力来保护服务器免受过载和攻击。第一点也是最重要的一点是，只有完整且有效的请求才会被转发到服务器。最初的原因是HAProxy需要找到它需要与字节流保持同步的协议元素，第二个原因是，在请求完成之前，没有办法知道某些元素是否会改变其语义。这样做的直接好处是服务器不会暴露给无效或不完整的请求。这是一种非常有效的防御慢速攻击的方法，这种攻击对HAProxy几乎没有影响。

另一个重要的一点是，HAProxy包含用于存储请求和响应的缓冲区，并且通过仅在请求完成时向服务器发送请求以及非常快速地从本地网络读取整个响应，服务器端连接的使用时间非常短，这尽可能地保留了服务器资源。

对此的直接扩展是，HAProxy可以人为地限制服务器的并发连接或未完成请求的数量，这保证了服务器永远不会过载，即使它在流量高峰期间连续以100%的容量运行。所有多余的请求都将排队等待在一个插槽被释放时处理。最后，这种巨大的资源节省通常确保了更好的服务器响应时间，最终实际上比服务器过载更快。排队的请求可能会被重新分配到其他服务器，甚至在客户端退出时在队列中终止，这也保护了服务器免受“重新加载效应”的影响，在缓慢加载的页面上，访问者每次点击“重新加载”通常都会引发一个新的请求，并使服务器处于过载状态。

慢启动机制还保护重新启动的服务器在完成启动或编译一些类时免受高流量的影响。

关于协议级保护，可以放松HTTP解析器以接受不符合标准但无害的请求或响应，甚至修复它们。这允许在开发修复程序时访问伪造的应用程序。与此同时，违规消息将被完整捕获，并提供详细的报告，帮助开发人员发现应用程序中的问题。最危险的协议违反得到了适当的检测、处理和修复。例如，具有两个Content-length头的格式错误的请求或响应，如果值完全相同，则会被固定，如果值不同则会被拒绝，因为这会成为一个安全问题。协议检测不仅限于HTTP，还可以用于其他协议，如TLS或RDP。

当检测到协议违反或攻击时，有各种选项可以响应用户，例如返回常见的“HTTP 400错误请求”，通过TCP重置关闭连接，或者在长时间延迟后伪造错误(“tarpit”)以迷惑攻击者。所有这些都有助于通过阻止违规客户端进行维护成本非常高的攻击来保护服务器。

HAProxy还提出了一些更高级的选项来防止意外的数据泄漏和会话交叉。它不仅可以记录可疑的服务器响应，而且还可以记录并选择性地阻止可能影响给定访问者机密性的响应。一个这样的例子是出现在可缓存响应中的可缓存cookie，它可能导致中间缓存将其传递给另一个访问者，从而导致意外的会话共享。
 
```



## [3.5.](http://docs.haproxy.org/2.7/intro.html#3.5) Advanced features



### [3.5.1.](http://docs.haproxy.org/2.7/intro.html#3.5.1) Advanced features : Management

```
HAProxy is designed to remain extremely stable and safe to manage in a regular production environment. It is provided as a single executable file which doesn't require any installation process. Multiple versions can easily coexist, meaning that it's possible (and recommended) to upgrade instances progressively by order of importance instead of migrating all of them at once. Configuration files are easily versioned. Configuration checking is done off-line so it doesn't require to restart a service that will possibly fail. During configuration checks, a number of advanced mistakes may be detected (e.g. a rule hiding another one, or stickiness that will not work) and detailed warnings and configuration hints are proposed to fix them. Backwards configuration file compatibility goes very far away in time, with version 1.5 still fully supporting configurations for versions 1.1 written 13 years before, and 1.6 only dropping support for almost unused, obsolete keywords that can be done differently. The configuration and software upgrade mechanism is smooth and non disruptive in that it allows old and new processes to coexist on the system,each handling its own connections. System status, build options, and library compatibility are reported on startup.

Some advanced features allow an application administrator to smoothly stop a server, detect when there's no activity on it anymore, then take it off-line,stop it, upgrade it and ensure it doesn't take any traffic while being upgraded,then test it again through the normal path without opening it to the public, and all of this without touching HAProxy at all. This ensures that even complicated production operations may be done during opening hours with all technical resources available.

The process tries to save resources as much as possible, uses memory pools to save on allocation time and limit memory fragmentation, releases payload buffers as soon as their contents are sent, and supports enforcing strong memory limits above which connections have to wait for a buffer to become available instead of allocating more memory. This system helps guarantee memory usage in certain strict environments.

A command line interface (CLI) is available as a UNIX or TCP socket, to perform a number of operations and to retrieve troubleshooting information. Everything done on this socket doesn't require a configuration change, so it is mostly used for temporary changes. Using this interface it is possible to change a server's address, weight and status, to consult statistics and clear counters, dump and clear stickiness tables, possibly selectively by key criteria, dump and kill client-side and server-side connections, dump captured errors with a detailed analysis of the exact cause and location of the error, dump, add and remove entries from ACLs and maps, update TLS shared secrets, apply connection limits and rate limits on the fly to arbitrary frontends (useful in shared hosting environments), and disable a specific frontend to release a listening port (useful when daytime operations are forbidden and a fix is needed nonetheless).
Updating certificates and their configuration on the fly is permitted, as well as enabling and consulting traces of every processing step of the traffic.

For environments where SNMP is mandatory, at least two agents exist, one is provided with the HAProxy sources and relies on the Net-SNMP Perl module.
Another one is provided with the commercial packages and doesn't require Perl.
Both are roughly equivalent in terms of coverage.

It is often recommended to install 4 utilities on the machine where HAProxy is deployed :

  - socat (in order to connect to the CLI, though certain forks of netcat can also do it to some extents);

  - halog from the latest HAProxy version : this is the log analysis tool, it parses native TCP and HTTP logs extremely fast (1 to 2 GB per second) and extracts useful information and statistics such as requests per URL, per source address, URLs sorted by response time or error rate, termination codes etc. It was designed to be deployed on the production servers to help troubleshoot live issues so it has to be there ready to be used;

  - tcpdump : this is highly recommended to take the network traces needed to troubleshoot an issue that was made visible in the logs. There is a moment where application and haproxy's analysis will diverge and the network traces are the only way to say who's right and who's wrong. It's also fairly common to detect bugs in network stacks and hypervisors thanks to tcpdump;

  - strace : it is tcpdump's companion. It will report what HAProxy really sees and will help sort out the issues the operating system is responsible for from the ones HAProxy is responsible for. Strace is often requested when a bug in HAProxy is suspected;
  
  
  HAProxy的设计是为了在常规生产环境中保持极其稳定和安全的管理。它作为单个可执行文件提供，不需要任何安装过程。多个版本可以轻松共存，这意味着可以(并且建议)按重要性顺序逐步升级实例，而不是一次性迁移所有实例。配置文件很容易进行版本控制。配置检查是离线完成的，因此不需要重新启动可能失败的服务。在配置检查期间，可能会检测到许多高级错误(例如，隐藏另一个规则的规则，或无法工作的粘性)，并提出详细的警告和配置提示来修复它们。向后配置文件的兼容性在时间上走得很远，版本1.5仍然完全支持13年前编写的版本1.1的配置，而1.6只放弃了对几乎未使用的、过时的关键字的支持，这些关键字可以用不同的方式完成。配置和软件升级机制是平滑和无中断的，因为它允许新旧进程在系统上共存，每个进程处理自己的连接。系统状态、构建选项和库兼容性在启动时报告。

一些高级功能允许应用程序管理员顺利地停止服务器，检测到服务器上不再有活动，然后将其离线，停止它，升级它并确保它在升级时不会占用任何流量，然后通过正常路径再次测试它，而不向公众开放它，所有这些都不需要触及HAProxy。这确保了即使是复杂的生产操作也可以在开放时间内用所有可用的技术资源完成。

该进程尝试尽可能地节省资源，使用内存池来节省分配时间和限制内存碎片，在内容发送后立即释放有效负载缓冲区，并支持强制执行强内存限制，超过该限制，连接必须等待缓冲区可用，而不是分配更多内存。该系统有助于在某些严格的环境下保证内存的使用。

命令行接口(CLI)作为UNIX或TCP套接字可用，用于执行许多操作和检索故障排除信息。在这个套接字上所做的一切都不需要更改配置，因此它主要用于临时更改。使用此接口可以更改服务器的地址，重量和状态，查询统计和清除计数器，转储和清除粘性表，可能有选择地根据关键标准，转储和杀死客户端和服务器端连接，转储捕获的错误，详细分析错误的确切原因和位置，转储，从acl和映射中添加和删除条目，更新TLS共享秘密，动态地对任意前端应用连接限制和速率限制(在共享主机环境中很有用)，并禁用特定前端以释放侦听端口(在禁止白天操作并且需要修复时很有用)。
允许动态更新证书及其配置，以及启用和查询流量的每个处理步骤的跟踪。

对于强制使用SNMP的环境，至少存在两个代理，其中一个与HAProxy源一起提供，并依赖于Net-SNMP Perl模块。
另一个是随商业包一起提供的，不需要Perl。
两者在覆盖范围上大致相当。

通常建议在部署HAProxy的机器上安装4个实用程序:

- socat(为了连接到CLI，尽管netcat的某些分支在某种程度上也可以这样做)

- halog来自最新的HAProxy版本:这是日志分析工具，它解析本地TCP和HTTP日志非常快(每秒1到2 GB)，并提取有用的信息和统计数据，如每个URL请求，每个源地址，按响应时间或错误率排序的URL，终止代码等。它被设计为部署在生产服务器上，以帮助解决实时问题，因此它必须准备好使用;

—tcpdump:强烈建议使用此方法进行网络跟踪，以便对日志中可见的问题进行故障排除。在某个时刻，应用程序和haproxy的分析将出现分歧，网络痕迹是判断谁对谁错的唯一方法。由于tcpdump，检测网络堆栈和管理程序中的错误也相当常见;

—strace: tcpdump的同伴。它将报告HAProxy真正看到的内容，并帮助将操作系统负责的问题与HAProxy负责的问题区分开来。当怀疑HAProxy存在bug时，通常会请求Strace;
 
```



### [3.5.2.](http://docs.haproxy.org/2.7/intro.html#3.5.2) Advanced features : System-specific capabilities

```
Depending on the operating system HAProxy is deployed on, certain extra features may be available or needed. While it is supported on a number of platforms,HAProxy is primarily developed on Linux, which explains why some features are only available on this platform.

The transparent bind and connect features, the support for binding connections to a specific network interface, as well as the ability to bind multiple processes to the same IP address and ports are only available on Linux and BSD systems, though only Linux performs a kernel-side load balancing of the incoming requests between the available processes.

On Linux, there are also a number of extra features and optimizations including support for network namespaces (also known as "containers") allowing HAProxy to be a gateway between all containers, the ability to set the MSS, Netfilter marks and IP TOS field on the client side connection, support for TCP FastOpen on the listening side, TCP user timeouts to let the kernel quickly kill connections when it detects the client has disappeared before the configured timeouts, TCP splicing to let the kernel forward data between the two sides of a connections thus avoiding multiple memory copies, the ability to enable the "defer-accept" bind option to only get notified of an incoming connection once data become available in the kernel buffers, and the ability to send the request with the ACK confirming a connect (sometimes called "piggy-back") which is enabled with the "tcp-smart-connect" option. On Linux, HAProxy also takes great care of manipulating the TCP delayed ACKs to save as many packets as possible on the network.

Some systems have an unreliable clock which jumps back and forth in the past and in the future. This used to happen with some NUMA systems where multiple processors didn't see the exact same time of day, and recently it became more common in virtualized environments where the virtual clock has no relation with the real clock, resulting in huge time jumps (sometimes up to 30 seconds have been observed). This causes a lot of trouble with respect to timeout enforcement in general. Due to this flaw of these systems, HAProxy maintains its own monotonic clock which is based on the system's clock but where drift is measured and compensated for. This ensures that even with a very bad system clock, timers remain reasonably accurate and timeouts continue to work. Note that this problem affects all the software running on such systems and is not specific to HAProxy.The common effects are spurious timeouts or application freezes. Thus if this behavior is detected on a system, it must be fixed, regardless of the fact that HAProxy protects itself against it.

On Linux, a new starting process may communicate with the previous one to reuse its listening file descriptors so that the listening sockets are never interrupted during the process's replacement.


根据部署HAProxy的操作系统的不同，某些额外的特性可能可用或需要。虽然HAProxy在许多平台上都得到支持，但它主要是在Linux上开发的，这就解释了为什么有些特性只在这个平台上可用。

透明的绑定和连接特性、对绑定连接到特定网络接口的支持，以及将多个进程绑定到相同IP地址和端口的能力，这些功能仅在Linux和BSD系统上可用，尽管只有Linux在可用进程之间执行传入请求的内核端负载平衡。

在Linux上，还有许多额外的功能和优化，包括支持网络名称空间(也称为“容器”)，允许HAProxy成为所有容器之间的网关，在客户端连接上设置MSS, Netfilter标记和IP TOS字段的能力，支持TCP侦听端FastOpen, TCP用户超时让内核在检测到客户端在配置的超时之前消失时快速终止连接。TCP拼接允许内核在连接的双方之间转发数据，从而避免了多个内存副本，启用“延迟-接受”绑定选项的能力，只有当内核缓冲区中的数据可用时才会收到传入连接的通知，以及使用“TCP -smart-connect”选项启用的带ACK确认连接(有时称为“背接”)的发送请求的能力。在Linux上，HAProxy还非常小心地处理TCP延迟的ack，以便在网络上保存尽可能多的数据包。

有些系统有一个不可靠的时钟，它在过去和未来之间来回跳跃。这种情况曾经发生在一些NUMA系统中，其中多个处理器没有看到一天中完全相同的时间，最近在虚拟环境中变得更加常见，其中虚拟时钟与真实时钟没有关系，导致巨大的时间跳变(有时已观察到长达30秒)。这通常会在超时执行方面造成很多麻烦。由于这些系统的缺陷，HAProxy维护自己的单调时钟，该时钟基于系统的时钟，但测量和补偿漂移。这确保了即使系统时钟非常糟糕，计时器仍然相当准确，超时继续工作。请注意，此问题会影响在此类系统上运行的所有软件，而不是HAProxy所特有的。常见的影响是虚假的超时或应用程序冻结。因此，如果在系统上检测到这种行为，则必须对其进行修复，而不管HAProxy是否保护自己不受其影响。

在Linux上，新的启动进程可以与前一个进程通信，以重用其侦听文件描述符，以便在进程替换期间侦听套接字永远不会中断。
 
```



### [3.5.3.](http://docs.haproxy.org/2.7/intro.html#3.5.3) Advanced features : Scripting

```
HAProxy can be built with support for the Lua embedded language, which opens a wide area of new possibilities related to complex manipulation of requests or responses, routing decisions, statistics processing and so on. Using Lua it is even possible to establish parallel connections to other servers to exchange information. This way it becomes possible (though complex) to develop an authentication system for example. Please refer to the documentation in the file "doc/lua-api/index.rst" for more information on how to use Lua.

HAProxy可以在支持Lua嵌入式语言的情况下构建，这为请求或响应的复杂操作、路由决策、统计处理等提供了广泛的新可能性。使用Lua甚至可以建立与其他服务器的并行连接以交换信息。这样就可以(尽管很复杂)开发例如身份验证系统。请参考文件“doc/lua-api/index”中的文档。有关如何使用Lua的更多信息，请访问。
```



### [3.5.4.](http://docs.haproxy.org/2.7/intro.html#3.5.4) Advanced features: Tracing

```
At any moment an administrator may connect over the CLI and enable tracing in various internal subsystems. Various levels of details are provided by default so that in practice anything between one line per request to 500 lines per request can be retrieved. Filters as well as an automatic capture on/off/pause mechanism are available so that it really is possible to wait for a certain event and watch it in detail. This is extremely convenient to diagnose protocol violations from faulty servers and clients, or denial of service attacks.


在任何时候，管理员都可以通过CLI连接并在各个内部子系统中启用跟踪。默认情况下提供了各种级别的详细信息，因此在实践中，每个请求可以检索一行到500行之间的任何内容。过滤器和自动捕获开/关/暂停机制都是可用的，因此可以真正等待某个事件并详细观察它。这对于从故障服务器和客户端诊断协议违反或拒绝服务攻击非常方便。
```



## [3.6.](http://docs.haproxy.org/2.7/intro.html#3.6) Sizing

```
Typical CPU usage figures show 15% of the processing time spent in HAProxy versus 85% in the kernel in TCP or HTTP close mode, and about 30% for HAProxy versus 70% for the kernel in HTTP keep-alive mode. This means that the operating system and its tuning have a strong impact on the global performance.

Usages vary a lot between users, some focus on bandwidth, other ones on request rate, others on connection concurrency, others on SSL performance. This section aims at providing a few elements to help with this task.

It is important to keep in mind that every operation comes with a cost, so each individual operation adds its overhead on top of the other ones, which may be negligible in certain circumstances, and which may dominate in other cases.

When processing the requests from a connection, we can say that :

  - forwarding data costs less than parsing request or response headers;

  - parsing request or response headers cost less than establishing then closing a connection to a server;

  - establishing an closing a connection costs less than a TLS resume operation;

  - a TLS resume operation costs less than a full TLS handshake with a key computation;

  - an idle connection costs less CPU than a connection whose buffers hold data;

  - a TLS context costs even more memory than a connection with data;

So in practice, it is cheaper to process payload bytes than header bytes, thus it is easier to achieve high network bandwidth with large objects (few requests per volume unit) than with small objects (many requests per volume unit). This explains why maximum bandwidth is always measured with large objects, while request rate or connection rates are measured with small objects.

Some operations scale well on multiple processes spread over multiple CPUs,and others don't scale as well. Network bandwidth doesn't scale very far because the CPU is rarely the bottleneck for large objects, it's mostly the network bandwidth and data buses to reach the network interfaces. The connection rate doesn't scale well over multiple processors due to a few locks in the system when dealing with the local ports table. The request rate over persistent connections scales very well as it doesn't involve much memory nor network bandwidth and doesn't require to access locked structures. TLS key computation scales very well as it's totally CPU-bound. TLS resume scales moderately well,but reaches its limits around 4 processes where the overhead of accessing the shared table offsets the small gains expected from more power.

The performance numbers one can expect from a very well tuned system are in the following range. It is important to take them as orders of magnitude and to expect significant variations in any direction based on the processor, IRQ setting, memory type, network interface type, operating system tuning and so on.

典型的CPU使用数据显示，在TCP或HTTP关闭模式下，HAProxy的处理时间占15%，而内核的处理时间占85%;在HTTP保持活动模式下，HAProxy的处理时间占30%，内核的处理时间占70%。这意味着操作系统及其调优对全局性能有很大的影响。

用户之间的用法差别很大，有些关注带宽，有些关注请求速率，有些关注连接并发性，有些关注SSL性能。本节旨在提供一些元素来帮助完成这项任务。

重要的是要记住，每个操作都有成本，因此每个单独的操作都会在其他操作的基础上增加其开销，在某些情况下可以忽略不计，而在其他情况下可能占主导地位。

在处理来自连接的请求时，我们可以说:

-转发数据成本低于解析请求或响应头;

解析请求或响应头的成本低于建立和关闭与服务器的连接;

-建立和关闭连接的成本低于TLS恢复操作;

-一个TLS恢复操作的成本低于一个完整的TLS握手和一个密钥计算;

一个空闲的连接比一个有数据的连接消耗更少的CPU;

- TLS上下文比数据连接消耗更多的内存;

因此，在实践中，处理有效负载字节比处理报头字节更便宜，因此使用大对象(每个卷单元请求很少)比使用小对象(每个卷单元请求很多)更容易实现高网络带宽。这解释了为什么总是用大对象来测量最大带宽，而用小对象来测量请求速率或连接速率。

一些操作在分布在多个cpu上的多个进程上可以很好地扩展，而另一些操作则不能很好地扩展。网络带宽不能扩展得太远，因为CPU很少是大型对象的瓶颈，主要是网络带宽和数据总线到达网络接口。在处理本地端口表时，由于系统中有一些锁，连接速率不能很好地扩展到多个处理器。持久连接的请求速率可以很好地扩展，因为它不涉及太多内存和网络带宽，也不需要访问锁定的结构。TLS密钥计算的扩展性非常好，因为它完全受cpu限制。TLS恢复的伸缩性还算好，但在4个进程左右达到极限，此时访问共享表的开销抵消了从更大的功率中预期获得的小收益。

一个经过良好调优的系统的性能数字在以下范围内。重要的是将它们视为数量级，并根据处理器、IRQ设置、内存类型、网络接口类型、操作系统调优等在任何方向上预期显著变化。
 



The following numbers were found on a Core i7 running at 3.7 GHz equipped with a dual-port 10 Gbps NICs running Linux kernel 3.10, HAProxy 1.6 and OpenSSL 1.0.2. HAProxy was running as a single process on a single dedicated CPU core,and two extra cores were dedicated to network interrupts :

  - 20 Gbps of maximum network bandwidth in clear text for objects 256 kB or higher, 10 Gbps for 41kB or higher;

  - 4.6 Gbps of TLS traffic using AES256-GCM cipher with large objects;

  - 83000 TCP connections per second from client to server;

  - 82000 HTTP connections per second from client to server;

  - 97000 HTTP requests per second in server-close mode (keep-alive with the client, close with the server);

  - 243000 HTTP requests per second in end-to-end keep-alive mode;

  - 300000 filtered TCP connections per second (anti-DDoS)

  - 160000 HTTPS requests per second in keep-alive mode over persistent TLS connections;

  - 13100 HTTPS requests per second using TLS resumed connections;

  - 1300 HTTPS connections per second using TLS connections renegotiated with RSA2048;

  - 20000 concurrent saturated connections per GB of RAM, including the memory  required for system buffers; it is possible to do better with careful tuning but this result it easy to achieve.

  - about 8000 concurrent TLS connections (client-side only) per GB of RAM,including the memory required for system buffers;

  - about 5000 concurrent end-to-end TLS connections (both sides) per GB of RAM including the memory required for system buffers;

A more recent benchmark featuring the multi-thread enabled HAProxy 2.4 on a 64-core ARM Graviton2 processor in AWS reached 2 million HTTPS requests per second at sub-millisecond response time, and 100 Gbps of traffic:

  https://www.haproxy.com/blog/haproxy-forwards-over-2-million-http-requests-per-second-on-a-single-aws-arm-instance/

Thus a good rule of thumb to keep in mind is that the request rate is divided by 10 between TLS keep-alive and TLS resume, and between TLS resume and TLS renegotiation, while it's only divided by 3 between HTTP keep-alive and HTTP close. Another good rule of thumb is to remember that a high frequency core with AES instructions can do around 20 Gbps of AES-GCM per core.

Another good rule of thumb is to consider that on the same server, HAProxy will be able to saturate :

  - about 5-10 static file servers or caching proxies;

  - about 100 anti-virus proxies;

  - and about 100-1000 application servers depending on the technology in use.
  
  
  以下数字是在3.7 GHz的酷睿i7上发现的，它配备了双端口10gbps网卡，运行Linux内核3.10、HAProxy 1.6和OpenSSL 1.0.2。HAProxy在一个专用CPU核心上作为单个进程运行，另外两个核心专门用于网络中断:

- 256 kB或更高的明文对象的最大网络带宽为20 Gbps, 41kB或更高的对象的最大网络带宽为10 Gbps;

- 4.6 Gbps的TLS流量使用AES256-GCM密码与大对象;

- 83000 TCP连接每秒从客户端到服务器;

- 82000 HTTP连接每秒从客户端到服务器;

-在服务器关闭模式下每秒97000个HTTP请求(与客户端保持连接，与服务器关闭)

-在端到端保持连接模式下每秒243000个HTTP请求;

—每秒过滤300,000个TCP连接(anti-DDoS)

-在持久的TLS连接上，每秒160000个HTTPS请求

-每秒13100个使用TLS恢复连接的HTTPS请求;

- 1300 HTTPS连接每秒使用TLS连接重新协商与RSA2048;

-每GB RAM有20000个并发饱和连接，包括系统缓冲区所需的内存;仔细调优是有可能做得更好的，但这个结果很容易实现。

-每GB RAM(包括系统缓冲区所需的内存)约8000个并发TLS连接(仅限客户端);

-每GB RAM(包括系统缓冲区所需的内存)约5000个并发端到端TLS连接(双方);

最近在AWS的64核ARM gravon2处理器上启用多线程的HAProxy 2.4的基准测试达到每秒200万个HTTPS请求，响应时间为亚毫秒，流量为100gbps。

https://www.haproxy.com/blog/haproxy-forwards-over-2-million-http-requests-per-second-on-a-single-aws-arm-instance/

因此，要记住的一个好的经验法则是，在TLS保持活动和TLS恢复之间，以及在TLS恢复和TLS重新协商之间，请求速率除以10，而在HTTP保持活动和HTTP关闭之间，请求速率仅除以3。另一个好的经验法则是记住，带有AES指令的高频核心每个核心可以实现大约20 Gbps的AES- gcm。

另一个好的经验法则是考虑在同一台服务器上，HAProxy将能够饱和:

-约5-10个静态文件服务器或缓存代理;

-约100个防毒代理程式;

-根据所使用的技术，大约有100-1000个应用服务器。
 
```



## [3.7.](http://docs.haproxy.org/2.7/intro.html#3.7) How to get HAProxy

```
HAProxy is an open source project covered by the GPLv2 license, meaning that everyone is allowed to redistribute it provided that access to the sources is also provided upon request, especially if any modifications were made.

HAProxy evolves as a main development branch called "master" or "mainline", from which new branches are derived once the code is considered stable. A lot of web sites run some development branches in production on a voluntarily basis, either to participate to the project or because they need a bleeding edge feature, and their feedback is highly valuable to fix bugs and judge the overall quality and stability of the version being developed.

The new branches that are created when the code is stable enough constitute a stable version and are generally maintained for several years, so that there is no emergency to migrate to a newer branch even when you're not on the latest.Once a stable branch is issued, it may only receive bug fixes, and very rarely minor feature updates when that makes users' life easier. All fixes that go into a stable branch necessarily come from the master branch. This guarantees that no fix will be lost after an upgrade. For this reason, if you fix a bug, please make the patch against the master branch, not the stable branch. You may even discover it was already fixed. This process also ensures that regressions in a stable branch are extremely rare, so there is never any excuse for not upgrading to the latest version in your current branch.

Branches are numbered with two digits delimited with a dot, such as "1.6".Since 1.9, branches with an odd second digit are mostly focused on sensitive technical updates and more aimed at advanced users because they are likely to trigger more bugs than the other ones. They are maintained for about a year only and must not be deployed where they cannot be rolled back in emergency. A complete version includes one or two sub-version numbers indicating the level of fix. For example, version 1.5.14 is the 14th fix release in branch 1.5 after version 1.5.0 was issued. It contains 126 fixes for individual bugs, 24 updates on the documentation, and 75 other backported patches, most of which were needed to fix the aforementioned 126 bugs. An existing feature may never be modified nor removed in a stable branch, in order to guarantee that upgrades within the same branch will always be harmless.



HAProxy是一个受GPLv2许可证保护的开源项目，这意味着每个人都可以重新发布它，只要可以根据请求访问源代码，特别是在进行任何修改的情况下。

HAProxy演变为一个称为“master”或“mainline”的主要开发分支，一旦代码被认为是稳定的，就会从中派生出新的分支。许多网站自愿在生产中运行一些开发分支，要么是为了参与项目，要么是因为他们需要一个前沿的功能，他们的反馈对于修复错误和判断正在开发的版本的整体质量和稳定性非常有价值。

当代码足够稳定时创建的新分支构成了一个稳定的版本，并且通常会维护数年，因此即使您没有使用最新的分支，也不会紧急迁移到较新的分支。一旦发布了一个稳定的分支，它可能只会收到错误修复，很少会收到小的功能更新，这让用户的生活更轻松。进入稳定分支的所有修复都必须来自主分支。这保证了在升级后不会丢失修复程序。由于这个原因，如果你修复了一个bug，请针对主分支而不是稳定分支做补丁。你甚至可能发现它已经被修复了。这个过程还确保了稳定分支中的回归是极其罕见的，所以没有任何借口不升级到当前分支中的最新版本。

分支用两个以点分隔的数字编号，例如“1.6”。从1.9开始，带有奇数第二位数字的分支主要关注敏感的技术更新，并且更多地针对高级用户，因为它们可能比其他分支触发更多的bug。它们只维持一年左右，不得在紧急情况下无法收回的情况下部署。一个完整的版本包括一个或两个子版本号，指示修复的级别。例如，版本1.5.14是1.5.0版本发布后分支1.5中的第14个修复版本。它包含针对单个错误的126个修复，文档上的24个更新，以及75个其他后移植补丁，其中大多数都需要修复前面提到的126个错误。在一个稳定的分支中，现有的功能可能永远不会被修改或删除，以保证同一分支中的升级总是无害的。
 

HAProxy is available from multiple sources, at different release rhythms :

  - The official community web site : http://www.haproxy.org/ : this site provides the sources of the latest development release, all stable releases,as well as nightly snapshots for each branch. The release cycle is not fast,several months between stable releases, or between development snapshots.
    Very old versions are still supported there. Everything is provided as sources only, so whatever comes from there needs to be rebuilt and/or repackaged;

  - GitHub : https://github.com/haproxy/haproxy/ : this is the mirror for the development branch only, which provides integration with the issue tracker,continuous integration and code coverage tools. This is exclusively for contributors;

  - A number of operating systems such as Linux distributions and BSD ports.These systems generally provide long-term maintained versions which do not always contain all the fixes from the official ones, but which at least contain the critical fixes. It often is a good option for most users who do  not seek advanced configurations and just want to keep updates easy;

  - Commercial versions from http://www.haproxy.com/ : these are supported  professional packages built for various operating systems or provided as appliances, based on the latest stable versions and including a number of features backported from the next release for which there is a strong  demand. It is the best option for users seeking the latest features with the reliability of a stable branch, the fastest response time to fix bugs,or simply support contracts on top of an open source product;

HAProxy可以从多个来源获得，以不同的释放节奏:

-官方社区网站:http://www.haproxy.org/:该网站提供了最新开发版本的来源，所有稳定版本，以及每个分支的夜间快照。发布周期并不快，稳定版本之间或开发快照之间可能需要几个月的时间。
那里仍然支持非常旧的版本。所有内容都只作为源代码提供，所以来自那里的任何内容都需要重新构建和/或重新打包;

- GitHub: https://github.com/haproxy/haproxy/:这是开发分支的镜像，它提供了与问题跟踪器、持续集成和代码覆盖工具的集成。这是专为贡献者;

—许多操作系统，如Linux发行版和BSD端口。这些系统通常提供长期维护的版本，这些版本并不总是包含来自官方版本的所有修复，但至少包含关键修复。对于大多数不寻求高级配置，只想保持更新简单的用户来说，这通常是一个不错的选择;

-来自http://www.haproxy.com/的商业版本:这些是支持为各种操作系统构建的专业软件包，或作为设备提供，基于最新的稳定版本，包括从下一个版本向后移植的许多功能，这些功能有强烈的需求。对于寻求最新特性的用户来说，它是最好的选择，具有稳定分支的可靠性，修复错误的最快响应时间，或者只是在开源产品之上支持合约;
 



In order to ensure that the version you're using is the latest one in your branch, you need to proceed this way :

  - verify which HAProxy executable you're running : some systems ship it by default and administrators install their versions somewhere else on the system, so it is important to verify in the startup scripts which one is used;

  - determine which source your HAProxy version comes from. For this, it's generally sufficient to type "haproxy -v". A development version will  appear like this, with the "dev" word after the branch number :

      HAProxy version 2.4-dev18-a5357c-137 2021/05/09 - https://haproxy.org/

    A stable version will appear like this, as well as unmodified stable versions provided by operating system vendors :

      HAProxy version 1.5.14 2015/07/02

    And a nightly snapshot of a stable version will appear like this with an hexadecimal sequence after the version, and with the date of the snapshot instead of the date of the release :

      HAProxy version 1.5.14-e4766ba 2015/07/29

    Any other format may indicate a system-specific package with its own patch set. For example HAProxy Enterprise versions will appear with the following format (<branch>-<latest commit>-<revision>) :

      HAProxy version 1.5.0-994126-357 2015/07/02

    Please note that historically versions prior to 2.4 used to report the process name with a hyphen between "HA" and "Proxy", including those above  which were adjusted to show the correct format only, so better ignore this word or use a relaxed match in scripts. Additionally, modern versions add  a URL linking to the project's home.

    Finally, versions 2.1 and above will include a "Status" line indicating  whether the version is safe for production or not, and if so, till when, as  well as a link to the list of known bugs affecting this version.

  - for system-specific packages, you have to check with your vendor's package repository or update system to ensure that your system is still supported,and that fixes are still provided for your branch. For community versions coming from haproxy.org, just visit the site, verify the status of your branch and compare the latest version with yours to see if you're on the latest one. If not you can upgrade. If your branch is not maintained anymore, you're definitely very late and will have to consider an upgrade to a more recent branch (carefully read the README when doing so).

HAProxy will have to be updated according to the source it came from. Usually it follows the system vendor's way of upgrading a package. If it was taken from sources, please read the README file in the sources directory after extracting the sources and follow the instructions for your operating system.

为了确保你使用的版本是分支中最新的版本，你需要这样做:

-验证你正在运行的是哪个HAProxy可执行文件:有些系统是默认的，管理员在系统的其他地方安装了他们的版本，所以在启动脚本中验证使用的是哪个版本是很重要的;

-确定您的HAProxy版本来自哪个源。为此，通常输入“haproxy -v”就足够了。开发版本将如下所示，在分支编号后面加上“dev”字:

HAProxy version 2.4-dev18-a5357c-137 2021/05/09 - https://haproxy.org/

一个稳定的版本会像这样出现，以及操作系统供应商提供的未修改的稳定版本:

HAProxy版本1.5.14 2015/07/02

稳定版本的夜间快照将以以下方式显示:版本后面有一个十六进制序列，并且快照日期而不是发布日期:

HAProxy版本1.5.14-e4766ba 2015/07/29

任何其他格式可能表示具有自己的补丁集的系统特定包。例如，HAProxy企业版本将以以下格式出现(&lt;branch&gt;-&lt;latest committ&gt;-&lt;revision&gt;):

HAProxy版本1.5.0-994126-357 2015/07/02

请注意，历史上2.4之前的版本使用在“HA”和“Proxy”之间使用连字符来报告进程名，包括上面调整为仅显示正确格式的那些，因此最好忽略该单词或在脚本中使用轻松匹配。此外，现代版本添加了一个链接到项目主页的URL。

最后，版本2.1及以上版本将包括一个“状态”行，指示该版本是否可以安全用于生产，如果是，直到什么时候，以及一个链接到影响该版本的已知错误列表。

-对于系统特定的软件包，您必须检查供应商的软件包存储库或更新系统，以确保您的系统仍然被支持，并且仍然为您的分支提供修复程序。对于来自haproxy.org的社区版本，只需访问该站点，验证您的分支的状态，并将最新版本与您的版本进行比较，看看您是否在使用最新版本。如果没有，你可以升级。如果您的分支不再维护，那么您肯定已经很晚了，并且必须考虑升级到最近的分支(在这样做时请仔细阅读README)。

HAProxy必须根据它的来源进行更新。通常它遵循系统供应商升级包的方法。如果它是从源代码中获取的，请在提取源代码后阅读源代码目录中的README文件，并按照操作系统的说明进行操作。
 

```



# [4.](http://docs.haproxy.org/2.7/intro.html#4) Companion products and alternatives

```
HAProxy integrates fairly well with certain products listed below, which is why they are mentioned here even if not directly related to HAProxy.

HAProxy与下面列出的某些产品集成得相当好，这就是为什么在这里提到它们，即使它们与HAProxy没有直接关联。
```



## [4.1.](http://docs.haproxy.org/2.7/intro.html#4.1) Apache HTTP server

```
Apache is the de-facto standard HTTP server. It's a very complete and modular project supporting both file serving and dynamic contents. It can serve as a frontend for some application servers. It can even proxy requests and cache responses. In all of these use cases, a front load balancer is commonly needed.
Apache can work in various modes, some being heavier than others. Certain modules still require the heavier pre-forked model and will prevent Apache from scaling well with a high number of connections. In this case HAProxy can provide a tremendous help by enforcing the per-server connection limits to a safe value and will significantly speed up the server and preserve its resources that will be better used by the application.

Apache can extract the client's address from the X-Forwarded-For header by using the "mod_rpaf" extension. HAProxy will automatically feed this header when "option forwardfor" is specified in its configuration. HAProxy may also offer a nice protection to Apache when exposed to the internet, where it will better resist a wide number of types of DoS attacks.


Apache是事实上的标准HTTP服务器。它是一个非常完整和模块化的项目，支持文件服务和动态内容。它可以作为一些应用服务器的前端。它甚至可以代理请求和缓存响应。在所有这些用例中，通常需要一个前端负载平衡器。
Apache可以在各种模式下工作，有些模式比其他模式更重。某些模块仍然需要较重的预分叉模型，并且会阻止Apache在大量连接的情况下进行良好的扩展。在这种情况下，HAProxy可以提供巨大的帮助，通过将每个服务器的连接限制强制到一个安全的值，并将显著提高服务器的速度，并保留其资源，这些资源将被应用程序更好地利用。

Apache可以通过使用“mod_rpaf”扩展名从X-Forwarded-For头中提取客户端的地址。当在配置中指定“option forwardfor”时，HAProxy将自动提供此报头。HAProxy还可以在暴露于互联网时为Apache提供很好的保护，从而更好地抵御各种类型的DoS攻击。
```



## [4.2.](http://docs.haproxy.org/2.7/intro.html#4.2) NGINX

```
NGINX is the second de-facto standard HTTP server. Just like Apache, it covers a wide range of features. NGINX is built on a similar model as HAProxy so it has no problem dealing with tens of thousands of concurrent connections. When used as a gateway to some applications (e.g. using the included PHP FPM) it can often be beneficial to set up some frontend connection limiting to reduce the load on the PHP application. HAProxy will clearly be useful there both as a regular load balancer and as the traffic regulator to speed up PHP by decongesting it. Also since both products use very little CPU thanks to their event-driven architecture, it's often easy to install both of them on the same system. NGINX implements HAProxy's PROXY protocol, thus it is easy for HAProxy to pass the client's connection information to NGINX so that the application gets all the relevant information. Some benchmarks have also shown that for large static file serving, implementing consistent hash on HAProxy in front of NGINX can be beneficial by optimizing the OS' cache hit ratio, which is basically multiplied by the number of server nodes.

NGINX是第二个事实上的标准HTTP服务器。就像Apache一样，它涵盖了广泛的特性。NGINX建立在与HAProxy类似的模型上，所以它处理成千上万的并发连接没有问题。当用作某些应用程序的网关时(例如使用包含的PHP FPM)，设置一些前端连接限制以减少PHP应用程序的负载通常是有益的。HAProxy作为常规的负载平衡器和流量调节器，通过减少拥塞来加快PHP的速度，显然是很有用的。此外，由于这两种产品都采用事件驱动的体系结构，因此使用的CPU很少，因此通常很容易在同一个系统上安装这两种产品。NGINX实现了HAProxy的PROXY协议，因此HAProxy很容易将客户端的连接信息传递给NGINX，从而使应用程序获得所有相关信息。一些基准测试也表明，对于大型静态文件服务，在NGINX前面的HAProxy上实现一致哈希可以通过优化操作系统的缓存命中率(基本上乘以服务器节点的数量)而受益。
```



## [4.3.](http://docs.haproxy.org/2.7/intro.html#4.3) Varnish

```
Varnish is a smart caching reverse-proxy, probably best described as a web application accelerator. Varnish doesn't implement SSL/TLS and wants to dedicate all of its CPU cycles to what it does best. Varnish also implements HAProxy's PROXY protocol so that HAProxy can very easily be deployed in front of Varnish as an SSL offloader as well as a load balancer and pass it all relevant client information. Also, Varnish naturally supports decompression from the cache when a server has provided a compressed object, but doesn't compress however. HAProxy can then be used to compress outgoing data when backend servers do not implement compression, though it's rarely a good idea to compress on the load balancer unless the traffic is low.

When building large caching farms across multiple nodes, HAProxy can make use of consistent URL hashing to intelligently distribute the load to the caching nodes and avoid cache duplication, resulting in a total cache size which is the sum of all caching nodes. In addition, caching of very small dumb objects for a short duration on HAProxy can sometimes save network round trips and reduce the CPU load on both the HAProxy and the Varnish nodes. This is only possible is no processing is done on these objects on Varnish (this is often referred to as the notion of "favicon cache", by which a sizeable percentage of useless downstream requests can sometimes be avoided). However do not enable HAProxy caching for a long time (more than a few seconds) in front of any other cache,that would significantly complicate troubleshooting without providing really significant savings.

Varnish是一个智能缓存反向代理，最好的描述是一个web应用程序加速器。Varnish没有实现SSL/TLS，它想把所有的CPU周期都用在它最擅长的事情上。Varnish还实现了HAProxy的代理协议，因此HAProxy可以很容易地部署在Varnish前面，作为SSL卸载器和负载平衡器，并向其传递所有相关的客户端信息。此外，当服务器提供压缩对象时，Varnish自然支持从缓存中解压，但不压缩。当后端服务器没有实现压缩时，HAProxy可以用来压缩传出的数据，尽管在负载平衡器上压缩数据并不是一个好主意，除非流量很低。

当构建跨多个节点的大型缓存农场时，HAProxy可以利用一致的URL散列来智能地将负载分配给缓存节点，避免缓存重复，从而使总缓存大小等于所有缓存节点的总和。此外，在HAProxy上短时间缓存非常小的哑对象有时可以节省网络往返，并减少HAProxy和Varnish节点上的CPU负载。这只有在Varnish上对这些对象不进行处理时才有可能(这通常被称为“favicon缓存”的概念，通过该概念，有时可以避免相当大比例的无用下游请求)。但是，不要在任何其他缓存前面长时间(超过几秒钟)启用HAProxy缓存，这会使故障排除变得非常复杂，而不会提供真正显著的节省。
```



## [4.4.](http://docs.haproxy.org/2.7/intro.html#4.4) Alternatives

```
Linux Virtual Server (LVS or IPVS) is the layer 4 load balancer included within the Linux kernel. It works at the packet level and handles TCP and UDP. In most cases it's more a complement than an alternative since it doesn't have layer 7 knowledge at all.

Pound is another well-known load balancer. It's much simpler and has much less features than HAProxy but for many very basic setups both can be used. Its author has always focused on code auditability first and wants to maintain the set of features low. Its thread-based architecture scales less well with high connection counts, but it's a good product.

Pen is a quite light load balancer. It supports SSL, maintains persistence using a fixed-size table of its clients' IP addresses. It supports a packet-oriented mode allowing it to support direct server return and UDP to some extents. It is meant for small loads (the persistence table only has 2048 entries).

NGINX can do some load balancing to some extents, though it's clearly not its primary function. Production traffic is used to detect server failures, the load balancing algorithms are more limited, and the stickiness is very limited.
But it can make sense in some simple deployment scenarios where it is already present. The good thing is that since it integrates very well with HAProxy,there's nothing wrong with adding HAProxy later when its limits have been reached.

Varnish also does some load balancing of its backend servers and does support real health checks. It doesn't implement stickiness however, so just like with NGINX, as long as stickiness is not needed that can be enough to start with.
And similarly, since HAProxy and Varnish integrate so well together, it's easy to add it later into the mix to complement the feature set.

Linux虚拟服务器(LVS或IPVS)是Linux内核中包含的第4层负载平衡器。它在数据包级别工作，处理TCP和UDP。在大多数情况下，它更像是一种补充而不是替代，因为它根本没有第7层知识。

Pound是另一个著名的负载平衡器。它比HAProxy简单得多，功能也少得多，但在许多非常基本的设置中，两者都可以使用。它的作者总是首先关注代码的可审核性，并希望保持低的特性集。它基于线程的架构在高连接数的情况下伸缩性不太好，但它是一个好产品。

Pen是一个非常轻量级的负载平衡器。它支持SSL，使用固定大小的客户端IP地址表维护持久性。它支持面向包的模式，允许它在一定程度上支持直接服务器返回和UDP。它适用于小负载(持久性表只有2048个条目)。

NGINX可以在某种程度上做一些负载平衡，尽管这显然不是它的主要功能。生产流量用于检测服务器故障，负载均衡算法受到更多限制，并且粘性非常有限。
但是在一些已经存在的简单部署场景中，它是有意义的。好的一面是，由于它与HAProxy集成得很好，所以在达到HAProxy的限制后再添加HAProxy并没有什么问题。

Varnish还对其后端服务器进行了一些负载平衡，并支持真正的健康检查。但是，它没有实现粘性，所以就像NGINX一样，只要不需要粘性，就可以开始。
同样，由于HAProxy和Varnish可以很好地集成在一起，所以以后很容易将其添加到组合中以补充功能集。
 
```



# [5.](http://docs.haproxy.org/2.7/intro.html#5) Contacts

```
If you want to contact the developers or any community member about anything, the best way to do it usually is via the mailing list by sending your message to haproxy@formilux.org. Please note that this list is public and its archives are public as well so you should avoid disclosing sensitive information. A thousand of users of various experience levels are present there and even the most complex questions usually find an optimal response relatively quickly.
Suggestions are welcome too. For users having difficulties with e-mail, a Discourse platform is available at http://discourse.haproxy.org/ . However please keep in mind that there are less people reading questions there and that most are handled by a really tiny team. In any case, please be patient and respectful with those who devote their spare time helping others.

I you believe you've found a bug but are not sure, it's best reported on the mailing list. If you're quite convinced you've found a bug, that your version is up-to-date in its branch, and you already have a GitHub account, feel free to go directly to https://github.com/haproxy/haproxy/ and file an issue with all possibly available details. Again, this is public so be careful not to post information you might later regret. Since the issue tracker presents itself as a very long thread, please avoid pasting very long dumps (a few hundreds lines or more) and attach them instead.

If you've found what you're absolutely certain can be considered a critical security issue that would put many users in serious trouble if discussed in a public place, then you can send it with the reproducer to security@haproxy.org.
A small team of trusted developers will receive it and will be able to propose a fix. We usually don't use embargoes and once a fix is available it gets merged. In some rare circumstances it can happen that a release is coordinated with software vendors. Please note that this process usually messes up with eveyone's work, and that rushed up releases can sometimes introduce new bugs,so it's best avoided unless strictly necessary; as such, there is often little consideration for reports that needlessly cause such extra burden, and the best way to see your work credited usually is to provide a working fix, which will appear in changelogs.

如果您想与开发人员或任何社区成员联系，最好的方法通常是通过邮件列表将您的消息发送到haproxy@formilux.org。请注意，此列表是公开的，其档案也是公开的，因此您应避免泄露敏感信息。这里有上千名不同经验水平的用户，即使是最复杂的问题通常也能相对较快地找到最佳答案。
也欢迎提出建议。对于使用电子邮件有困难的用户，可以在http://discourse.haproxy.org/上找到Discourse平台。但是请记住，阅读问题的人很少，而且大多数问题都是由一个非常小的团队处理的。在任何情况下，请耐心和尊重那些贡献自己的业余时间帮助别人。

如果您认为您已经发现了一个错误，但不确定，最好在邮件列表中报告。如果你确信你发现了一个bug，你的版本在分支中是最新的，并且你已经有一个GitHub帐户，可以直接访问https://github.com/haproxy/haproxy/并提交所有可能可用的细节问题。再说一次，这是公开的，所以要小心不要发布你以后可能会后悔的信息。由于问题跟踪器显示为一个很长的线程，请避免粘贴很长的转储文件(几百行或更多)，而是附加它们。

如果您发现了可以确定的关键安全问题，如果在公共场所讨论，将会给许多用户带来严重的麻烦，那么您可以将它与复制器一起发送到security@haproxy.org。
一个由受信任的开发人员组成的小团队将收到它，并能够提出修复方案。我们通常不使用禁运，一旦修复可用，它就会被合并。在某些罕见的情况下，可能会发生与软件供应商协调发布的情况。请注意，这个过程通常会打乱每个人的工作，而且匆忙发布的版本有时会引入新的bug，所以除非有必要，否则最好避免这样做;因此，通常很少考虑不必要地造成这种额外负担的报告，而查看工作记录的最佳方法通常是提供一个工作修复，它将出现在更改日志中。
 
```