# DHCPv6

**DHCPv6**是一个用来配置工作在[IPv6](https://zh.wikipedia.org/wiki/IPv6 "IPv6")网络上的IPv6[主机](https://zh.wikipedia.org/wiki/%E4%B8%BB%E6%A9%9F "主机")所需的[IP地址](https://zh.wikipedia.org/wiki/IP%E5%9C%B0%E5%9D%80 "IP地址")、IP前缀和/或其他配置的[网络协议](https://zh.wikipedia.org/wiki/%E7%BD%91%E7%BB%9C%E5%8D%8F%E8%AE%AE "网络协议")。

IPv6主机可以使用[无状态地址自动配置](https://zh.wikipedia.org/wiki/IPv6#SLAAC "IPv6")（SLAAC）或DHCPv6来获得IP地址。DHCP倾向于被用在需要集中管理主机的站点，而无状态自动配置不需要任何集中管理，因此后者更多地被用在典型家庭网络这样的场景下。

使用无状态自动配置的IPv6主机可能会需要除了IP地址以外的其他信息。DHCPv6可被用来获取这样的信息，哪怕这些信息对于配置IP地址毫无用处。配置[DNS](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F "域名系统")服务器无需使用DHCPv6，它们可以使用无状态自动配置所需的[邻居发现协议](https://zh.wikipedia.org/wiki/%E9%82%BB%E5%B1%85%E5%8F%91%E7%8E%B0%E5%8D%8F%E8%AE%AE "邻居发现协议")来进行配置[[1]](https://zh.wikipedia.org/zh-hans/DHCPv6#cite_note-1)。

IPv6路由器，如家庭路由器，必须在无需人工干预的情况下被自动配置。这样的路由器不仅需要一个IPv6地址用来与上游路由器通信，还需要一个IPv6前缀用来配置下游的设备。DHCPv6 [前缀代理](https://zh.wikipedia.org/wiki/%E5%89%8D%E7%BC%80%E4%BB%A3%E7%90%86 "前缀代理")提供了配置此类路由器的机制。

## 实现

### 端口号

DHCPv6客户端使用UDP端口号546，服务器使用端口号547。

### DHCP唯一标识符

DHCP唯一标识符（DUID）用于客户端从DHCPv6服务器获得IP地址。最小长度为12个字节（96位），最大长度为20字节（160位）。实际长度取决于其类型。服务器将DUID与其数据库进行比较，并将配置数据（地址、租期、DNS服务器，等等）发送给客户端。DUID的前16位包含了DUID的三种类型之一。剩余的96位取决于DUID类型。

### 举例

本例中，服务器的链路本地地址是`fe80::0011:22ff:fe33:5566`，客户端的链路本地地址是`fe80::aabb:ccff:fedd:eeff`。

- DHCPv6客户端从`[fe80::aabb:ccff:fedd:eeff]:546`发送**Solicit**至`[ff02::1:2]:547`。
- DHCPv6服务器从`[fe80::0011:22ff:fe33:5566]:547`回应一个**Advertise**给`[fe80::aabb:ccff:fedd:eeff]:546`。
- DHCPv6客户端从`[fe80::aabb:ccff:fedd:eeff]:546`回应一个**Request**给`[ff02::1:2]:547`。（依照[RFC 8415](http://tools.ietf.org/html/rfc8415)（[页面存档备份](https://web.archive.org/web/20191127185120/http://tools.ietf.org/html/rfc8415)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%A1%A3%E6%A1%88%E9%A6%86 "互联网档案馆")）的[section 14](http://tools.ietf.org/html/rfc8415#section-14)（[页面存档备份](https://web.archive.org/web/20191127185120/http://tools.ietf.org/html/rfc8415#section-14)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%A1%A3%E6%A1%88%E9%A6%86 "互联网档案馆")），所有客户端消息都发送到多播地址)
- DHCPv6服务器以`[fe80::0011:22ff:fe33:5566]:547`到`[fe80::aabb:ccff:fedd:eeff]:546`的**Reply**结束。

## IETF标准

- [RFC 3315](https://tools.ietf.org/html/rfc3315), "Dynamic Host Configuration Protocol for IPv6 (DHCPv6)"
- [RFC 3319](https://tools.ietf.org/html/rfc3319), "Dynamic Host Configuration Protocol (DHCPv6) Options for Session Initiation Protocol (SIP) Servers"
- [RFC 3633](https://tools.ietf.org/html/rfc3633), "IPv6 Prefix Options for Dynamic Host Configuration Protocol (DHCP) version 6"
- [RFC 3646](https://tools.ietf.org/html/rfc3646), "DNS Configuration options for Dynamic Host Configuration Protocol for IPv6 (DHCPv6)"
- [RFC 3736](https://tools.ietf.org/html/rfc3736), "Stateless Dynamic Host Configuration Protocol (DHCP) Service for IPv6"
- [RFC 5007](https://tools.ietf.org/html/rfc5007), "DHCPv6 Leasequery"
- [RFC 6221](https://tools.ietf.org/html/rfc6221), "Lightweight DHCPv6 Relay Agent"
- [RFC 6355](https://tools.ietf.org/html/rfc6355), "Definition of the UUID-Based DHCPv6 Unique Identifier (DUID-UUID)"
- [RFC 6939](https://tools.ietf.org/html/rfc6939), "Client Link-Layer Address Option in DHCPv6"
- [RFC 8415](https://tools.ietf.org/html/rfc8415), "Dynamic Host Configuration Protocol for IPv6 (DHCPv6)" - Obsoletes [RFC 3315](https://tools.ietf.org/html/rfc3315), [RFC 3633](https://tools.ietf.org/html/rfc3633), [RFC 3736](https://tools.ietf.org/html/rfc3736), [RFC 4242](https://tools.ietf.org/html/rfc4242), [RFC 7083](https://tools.ietf.org/html/rfc7083), [RFC 7283](https://tools.ietf.org/html/rfc7283), [RFC 7550](https://tools.ietf.org/html/rfc7550).

## 参考资料

1. [RFC 4339](https://tools.ietf.org/html/rfc4339), _IPv6 Host Configuration of DNS Server Information Approaches_, J. Jeong (February 2006)

## 外部链接

- [IPv6 Intelligence: DHCPv6](http://ipv6int.net/software/index.html#dhcpv6)（[页面存档备份](https://web.archive.org/web/20130507060919/http://ipv6int.net/software/index.html#dhcpv6)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%A1%A3%E6%A1%88%E9%A6%86 "互联网档案馆")）, comparison of DHCPv6 packages and implementations (Last updated: April, 2009)
- [IPv6 Ready: DHCPv6](https://www.ipv6ready.org/db/index.php/public/search/?l=&c=&ds=&de=&pc=&ap=2&oem=&etc=D&fw=&vn=&do=1&o=6)（[页面存档备份](https://web.archive.org/web/20140201185746/https://www.ipv6ready.org/db/index.php/public/search/?l=&c=&ds=&de=&pc=&ap=2&oem=&etc=D&fw=&vn=&do=1&o=6)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%A1%A3%E6%A1%88%E9%A6%86 "互联网档案馆")）, list of IPv6 Phase II Certified DHCPv6 implementations (Last updated: December, 2012)

|   |
|---|
|\|[[隐藏](https://zh.wikipedia.org/zh-hans/DHCPv6#)]<br><br>- [查](https://zh.wikipedia.org/wiki/Template:IPv6 "Template:IPv6")<br>- [论](https://zh.wikipedia.org/w/index.php?title=Template_talk:IPv6&action=edit&redlink=1 "Template talk:IPv6（页面不存在）")<br>- [编](https://zh.wikipedia.org/w/index.php?title=Template:IPv6&action=edit)<br><br>[网际协议第6版](https://zh.wikipedia.org/wiki/IPv6 "IPv6")\|   \|<br>\|---\|---\|<br>\|   \|   \|<br>\|概述\|- [IPv6](https://zh.wikipedia.org/wiki/IPv6 "IPv6")<br>- [IPv6地址](https://zh.wikipedia.org/wiki/IPv6 "IPv6")<br>- [IPv6报文](https://zh.wikipedia.org/w/index.php?title=IPv6%E6%8A%A5%E6%96%87&action=edit&redlink=1)<br>- [移动IPv6](https://zh.wikipedia.org/wiki/%E7%A7%BB%E5%8A%A8IP "移动IP")\|<br>\|   \|   \|<br>\|部署\|- [IPv6部署](https://zh.wikipedia.org/wiki/IPv6%E9%83%A8%E7%BD%B2 "IPv6部署")<br>- [IPv6应用程序支持情况比较](https://zh.wikipedia.org/w/index.php?title=IPv6%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E6%94%AF%E6%8C%81%E6%83%85%E5%86%B5%E6%AF%94%E8%BE%83&action=edit&redlink=1)<br>- [各操作系统IPv6支持情况比较](https://zh.wikipedia.org/wiki/%E5%90%84%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9FIPv6%E6%94%AF%E6%8C%81%E6%83%85%E5%86%B5%E6%AF%94%E8%BE%83 "各操作系统IPv6支持情况比较")<br>- [IPv6隧道中间人列表](https://zh.wikipedia.org/wiki/IPv6%E9%9A%A7%E9%81%93%E4%B8%AD%E9%97%B4%E4%BA%BA%E5%88%97%E8%A1%A8 "IPv6隧道中间人列表")<br>- [世界IPv6日](https://zh.wikipedia.org/wiki/%E4%B8%96%E7%95%8CIPv6%E6%97%A5 "世界IPv6日")\|<br>\|   \|   \|<br>\|IPv4转IPv6主题\|- [IPv4地址枯竭](https://zh.wikipedia.org/wiki/IPv4%E4%BD%8D%E5%9D%80%E6%9E%AF%E7%AB%AD "IPv4位址枯竭")<br>- [IPv6过渡机制](https://zh.wikipedia.org/wiki/IPv6%E8%BF%87%E6%B8%A1%E6%9C%BA%E5%88%B6 "IPv6过渡机制")<br>- [6in4](https://zh.wikipedia.org/wiki/6in4 "6in4")<br>- [6to4](https://zh.wikipedia.org/wiki/6to4 "6to4")<br>- [6over4](https://zh.wikipedia.org/wiki/6over4 "6over4")<br>- [ISATAP](https://zh.wikipedia.org/wiki/ISATAP "ISATAP")<br>- [Teredo](https://zh.wikipedia.org/wiki/Teredo%E9%9A%A7%E9%81%93 "Teredo隧道")\|<br>\|   \|   \|<br>\|IPv6转IPv4主题\|- [DNS64](https://zh.wikipedia.org/wiki/DNS64 "DNS64")<br>- [NAT64](https://zh.wikipedia.org/wiki/NAT64 "NAT64")\|<br>\|   \|   \|<br>\|相关协议\|- DHCPv6<br>- [ICMPv6](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE%E7%AC%AC%E5%85%AD%E7%89%88 "互联网控制消息协议第六版")<br>    - [邻居发现协议](https://zh.wikipedia.org/wiki/%E9%82%BB%E5%B1%85%E5%8F%91%E7%8E%B0%E5%8D%8F%E8%AE%AE "邻居发现协议")<br>    - [安全邻居发现协议](https://zh.wikipedia.org/wiki/%E5%AE%89%E5%85%A8%E9%82%BB%E5%B1%85%E5%8F%91%E7%8E%B0%E5%8D%8F%E8%AE%AE "安全邻居发现协议")<br>    - [多播路由器发现](https://zh.wikipedia.org/w/index.php?title=%E5%A4%9A%E6%92%AD%E8%B7%AF%E7%94%B1%E5%99%A8%E5%8F%91%E7%8E%B0&action=edit&redlink=1)<br>- [IPv6中介的网站多宿主](https://zh.wikipedia.org/w/index.php?title=IPv6%E4%B8%AD%E4%BB%8B%E7%9A%84%E7%BD%91%E7%AB%99%E5%A4%9A%E5%AE%BF%E4%B8%BB&action=edit&redlink=1)\||

[分类](https://zh.wikipedia.org/wiki/Special:%E9%A1%B5%E9%9D%A2%E5%88%86%E7%B1%BB "Special:页面分类")：​

- [IPv6](https://zh.wikipedia.org/wiki/Category:IPv6 "Category:IPv6")
- [应用层协议](https://zh.wikipedia.org/wiki/Category:%E5%BA%94%E7%94%A8%E5%B1%82%E5%8D%8F%E8%AE%AE "Category:应用层协议")