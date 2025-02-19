---
title: "VPC 网络简介"
linkTitle: "VPC 网络简介"
description:
draft: false
weight: 10
---

## 产品概述

VPC 网络，即虚拟专属私有网络（Virtual Private Cloud，以下简称 VPC 网络），是山河提供的云上私有网络空间，为用户在云上创建的云资源提供隔离的虚拟网络环境。不同VPC网络之间逻辑隔离，以保障用户云上资源的安全性。在 VPC 网络内，您可以自定义 IP 地址段、路由表、安全策略等，快速部署及灵活管理属于自己的云上网络。

## 产品优势

- **超大规模**

  一个 VPC 网络可以连接254个私有网络（Vxnet），每个私有网络最多可以容纳252台云服务器，每个 VPC 可以支持60000+云服务器。所以，VPC 网络可以承载超大规模的用户业务。

- **安全可靠**

  支持同区域跨可用区容灾。在由多个可用区（Availability Zone）组成的区域中创建的 VPC 网络 ，默认支持多可用区部署（ Multi-AZ deployment，简称 MZ ），可实现同一个私有网络的云服务器位于不同的可用区节点。在单个可用区的业务故障后，可以配合负载均衡器和虚拟IP将业务快速切换到正常的可用区，切换过程不用更换私有网络及业务IP地址。

- **超高性能**

  VPC网络采用分布式路由器和虚拟二层链路技术，采用智能网卡对网络负载进行卸载，可实现超高性能的网络性能，满足有严苛性能的业务需求。

- **全面支持虚拟IP**

  虚拟 IP通常用于不同云服务器之间的切换，例如主从切换，是业务连续性的重要基础。VPC 内所有IP地址均可以作为虚拟 IP 使用，并且支持对虚拟IP的预留申请及查看功能。

- **支持组播广播**

  在 VPC 中创建的私有网络是一个二层网络，私有网络中的云服务器可以通过“广播”功能和全网内的其他云服务器通信，或以广播的方式组建组播。

- **跨用户共享**

  VPC 网络支持通过“项目”方式与其他用户共享，实现跨用户协同。

- **图形化操作**

  支持图形化查看和编辑网络拓扑，可让用户对整体网络一目了然，形成形象认知。

## 产品功能

VPC 网络拥有众多的二层三层网络功能，以及隧道、VPN 等高级功能。

- **VPC网络**

  提供管理 VPC 网络的功能，包括：创建 VPC 网络、修改 VPC 网络基本信息、删除VPC网络、VPC 健康探测、为 VPC 网络绑定告警策略和将 VPC 网络添加到项目。

- **私有网络**

  私有网络（Vxnet）是 VPC 网络中的子网，用于云服务器之间互联，它类似物理交换机 (L2Switch) 组成的局域网。不同用户的私有网络之间100%隔离。私有网络是一个二层网络，在私有网络中的云服务器可以通过“广播”功能和全网内的其他云服务器通信，也可以接收到来自其他云服务器的广播。私有网络支持多种功能，为用户提供高度的自定义网络：DHCP、IPv4/IPv6双栈、网络 ACL、路由表、虚拟IP管理、跨可用区多活、自定义网关/边界路由地址。

- **路由表**

  创建VPC网络的同时，会自动创建一个一定规格（支持小型/中型/大型/超大型）的虚拟路由器，路由器具备一个私有的B段（例如192.168.0.0/24）网络地址空间。在路由器中有默认的直联路由表，以保证同一个私有网络下的所有子网互通。用户也在路由器中自定义路由表，并且选择应用到私有网络中。山河提供了管理路由表的功能：添加自定义路由、查询路由、修改路由和删除路由等。

- **地址转换（NAT）**

  NAT ( Network Address Translation )是将一个公网IP转换为很多个内网IP的技术，最常见的IP复用技术之一。山河提供的 VPC 网络支持源地址转换（SNAT）和目的地址转换（DNAT）。

- **网络隧道**

  网络隧道通过加密/非加密通道将多地或者多个网络安全连接起来，例如连接企业数据中心、企业办公网络、 Internet 终端以及VPC 和 VPC互通 。

  山河支持2种网络隧道：IPsec隧道和GRE隧道。IPsec 是一种加密的隧道技术，通过使用加密的安全服务在不同的网络之间建立保密而安全的通讯隧道。GRE 隧道是 L3 over L3 的技术，不对传输数据加密，但是使用起来更加简单灵活，可以用来组建各种网络拓扑，常见的树型、星型、总线型(串型)、环型和混合型均可实现。

- **VPN服务**

  VPN 服务是基于 Internet 的传输加密方式，在单台机器而非整个网络连接到 VPC 网络时可以通过密码或者证书的方式简单实现。

  山河支持 OpenVPN、PPTP、 L2TP 三种不同的VPN服务，用户可以根据自身的使用习惯进行选择。

- **DNS服务**

  VPC网络为用户提供内网 DNS 和公网 DNS 转发服务。内网DNS可以为私有网络中的资源绑定域名，支持一个域名绑定多个私网IP。在 VPC 绑定公网IP后，可以为其中的私有网络提供公网 DNS 转发服务，未指定公网DNS转发时将采用山河平台的默认配置。

- **路由推送**

  VPC 还可以为其中的每个云服务器推送静态/默认路由，云服务器路由在所有的路由中优先级最高，用户在自定义云服务器路由时应确保设定正确，以免网络不通。

- **网关过滤控制（ACL）**

  网关过滤控制仅对虚拟路由器中的流量进行控制，包含 NAT、隧道、VPN 流量会经过虚拟路由器。虚拟网络内部，或者虚拟网络与虚拟网络间的流量均不经过虚拟路由器。虚拟网络内部的流量可以通过安全组进行访问控制，虚拟网络之间的流量通过网络 ACL 来进行访问控制。

