**# IPv6系列-彻底弄明白有状态与无状态配置IPv6地址

作者：小慢哥本人2019-10-09 08:29:30

[网络](https://www.51cto.com/network)[网络管理](https://www.51cto.com/management)

何时采用无状态、何时采用有状态，关键看应用场景。核心在于是否需要控制IP地址，比如保持IP不变，如果需要控制，就采用有状态；如果无需控制，就采用无状态。

**一、Link-Local Address的生成方式**

生成“链路本地地址”，有2种方式：

- 手动配置
- 自动配置

其中“自动配置”根据算法，又分为：

- eui64：根据mac地址换算而来
- stable_secret：跟随网络环境的变化而变化，处于固定网络环境时其值将固定
- random：随机生成

**二、Global Address的生成方式**

生成“全球单播地址”(或者“唯一本地地址”)，有2种方式：

- 手动配置
- 自动配置

其中“自动配置”根据获取方式，又分为

- 无状态(Stateless)：根据路由通告报文RA(Router Advertisement)包含的prefix前缀信息自动配置IPv6地址，组成方式是Prefix + (EUI64 or 随机)。Stateless也可以称为SLAAC(Stateless address autoconfiguration)
- 有状态(Stateful)：通过DHCPv6方式获得IPv6地址

其中“有状态”又分为2种：

- 有状态DHCPv6(Stateful DHCPv6)：IPv6地址、其他参数(如DNS)均通过DHCPv6获取
- 无状态DHCPv6(Stateless DHCPv6)：IPv6地址依然通过路由通告RA方式生成，其他参数(如DNS)通过DHCPv6获取

为了避免混淆，在此解释下有状态、无状态到底是什么意思：首先，请明确一点，有状态、无状态仅针对于IPv6地址分配方式，并不包含其他参数

- 有状态：可控、可管理。在网络中存在一个IP地址管理者，它能够识别客户端，根据不同的客户端，分配对应的IPv6地址，客户端与服务端之间需要维护IP地址的租期及续约。目前实现这种效果的，就是DHCPv6协议，IP地址管理者就是DHCPv6 Server
- 无状态：不可控、难管理。在网络中只有网关，没有IP地址管理者。因此无人去识别客户端，每个客户端根据网关发送的相同的RA报文内容，自行配置IPv6地址

**三、RA报文中3个关键的Flag**

RA报文中存在3个关键的flag bit：

[![](https://cdn.nlark.com/yuque/0/2023/jpeg/34846357/1699251818442-aa774534-8f53-4cfa-8fe4-5013946f82ce.jpeg)
](https://s2.51cto.com/oss/201910/09/7d4874198a9516c6411276d5d35cceec.jpg)
![[7d4874198a9516c6411276d5d35cceec.jpg]]
(1) Autonomous flag(简称A flag)：表示是否配置无状态IP。在一个RA报文中，可存在多个prefix，比如2401::/64、2402::/64、2403::/64，每个prefix都可以独立配置A flag

- 为on时(对应bit位为1)：表示客户端应当在该prefix范围内自动生成IPv6地址(客户端通过DAD自行保证地址可用)，并配置子网路由条目、网关
- 为off时(对应bit位为0)：表示客户端不应当在该prefix范围内自动生成IPv6地址，但是可以配置子网路由条目、网关

(2) Managed flag(简称M flag)：表示是否配置有状态IP。M flag是RA报文的全局参数，一个RA报文只有一个M flag

- 为on时(对应bit位为1)：表示在stateless流程结束后开始stateful流程，也就是告诉客户端可以通过DHCPv6来获得IPv6地址和其他参数(如DNS列表)
- 为off时(对应bit位为0)：表示不通过DHCPv6来获得IPv6地址。

(3) Other flag(简称O flag)：表示是否通过DHCPv6获得除IP以外的其他参数(如DNS列表)。O flag也是RA报文中的全局参数，一个RA报文只有一个O flag。注意：仅当M flag为off时，该参数才会被读取。

- 为on时(对应bit位为1)：当M flag为on，或者M flag为off且至少有一个A flag为on时，将通过DHCPv6获得其他参数
- 为off时(对应bit位为0)：当M flag为on时，依然将通过DHCPv6获得其他参数;当M flag也为off时，将不通过DHCPv6获得其他参数

**四、流程示意图**

无状态和有状态并不是相互对立的，他们可以同时存在，也就是一张网卡上可以同时出现通过RA生成的IP以及通过DHCPv6获得的IP。通过下面这张笔者绘制的流程图可知晓其中奥秘。

[![](https://cdn.nlark.com/yuque/0/2023/jpeg/34846357/1699251818560-f1239827-acda-44f3-9013-4a0fa5a2e0e3.jpeg)](https://s5.51cto.com/oss/201910/09/30f14963ed65ecf48daf8ad758aaa251.jpg)

从图中可以看到，顺序为：

- Stateless自动配置“链路本地地址”
- Stateless自动配置“全球地址”(或“唯一本地地址”)
- Stateful自动配置“全球地址”(或“唯一本地地址”)和其他参数，其中Stateful阶段中存在Stateful DHCPv6或Stateless DHCPv6

注意：部分客户端操作系统或网络管理器当Stateless阶段没有收到RA报文后，就到此结束，不会走Stateful阶段，比如CentOS 7、Ubuntu 17的默认逻辑都是这样，而windows server 2012就会继续走Stateful阶段。

**五、测试获得IP效果**

测试环境：客户端基于CentOS 7+NetworkManager(即系统默认的网络管理方式)进行测试：

- 网关会发送RA报文，包含一个prefix
- DHCPv6 Server会分配IP、DNS

测试内容：测试M、O、A flag在所有排列组合的情况下：

- 客户端是否会通过RA报文配置无状态IP
- 客户端是否会通过RA报文配置prefix子网路由
- 客户端是否会通过RA报文配置gateway
- 客户端是否会通过DHCPv6获得有状态IP
- 客户端是否会通过DHCPv6获得DNS

测试结果：

[![](https://cdn.nlark.com/yuque/0/2023/jpeg/34846357/1699251818543-79728d52-146c-47cd-8b17-0806b595de31.jpeg)](https://s4.51cto.com/oss/201910/09/fdb649e1a4bc1dae34e23297fda50a70.jpg)

**六、应用场景(选择无状态还是有状态)**

何时采用无状态、何时采用有状态，关键看应用场景。核心在于是否需要控制IP地址，比如保持IP不变，如果需要控制，就采用有状态;如果无需控制，就采用无状态。

- 服务端领域：如对外提供服务，通常需要采用有状态IP。因为业务IP的突然变化容易导致业务中断(除非做好服务发现)
- 客户端领域：如移动设备、办公室内PC机，只需要上IPv6互联网，并不需要对外提供服务，可以采用无状态IP

——转载自[https://www.51cto.com/article/603898.htm](https://www.51cto.com/article/603898.html)**