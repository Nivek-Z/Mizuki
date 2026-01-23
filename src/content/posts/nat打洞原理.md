---
title: nat打洞
published: 2025-01-20
pinned: false
description: nat打洞
tags: [内网穿透]
category: 未分类
licenseName: "Unlicensed"
draft: false
date: 2025-01-20
pubDate: 2025-01-20
---


参考[【建议收藏】一次搞懂所有 NAT 类型，实战各NAT类型之间的打洞可行性，通过p2p vpn实现无公网ip穿透家庭内网，p2p直连打洞，游戏联机，BT下载 - YouTube](https://www.youtube.com/watch?v=T8Fx9b8z5w0&t=714s)

在线检测NAT类型：[https://natchecker.com](https://bulianglin.com/g/aHR0cHM6Ly9uYXRjaGVja2VyLmNvbS96aA)

上面那个不知道为什么跳转的网页不一样。。建议手动复制，难道被污染了。。？

感觉双nat4想要p2p直连是没希望了。。。

几种目前找到的方案：

-   关闭光猫防火墙并开启ipv6，然后用公网ipv6进行ddns到cloudflare等平台（需要想办法搞定光猫超级管理员权限）移动光猫可参考
    
-   使用类似frp的服务（需要公网中转服务器）
    

**下面是视频内容**[【建议收藏】一次搞懂所有 NAT 类型，实战各NAT类型之间的打洞可行性，通过p2p vpn实现无公网ip穿透家庭内网，p2p直连打洞，游戏联机，BT下载 - YouTube](https://www.youtube.com/watch?v=T8Fx9b8z5w0&t=714s)**总结**

由Gemini总结：

这是一份关于视频\*\*《【建议收藏】一次搞懂所有 NAT 类型，实战各NAT类型之间的打洞可行性》\*\*的详细总结。该视频由“不良林”发布，深入浅出地讲解了 NAT 的原理、不同类型的差异以及如何通过 P2P 技术实现内网穿透。

### **一、 NAT 的核心原理：映射与过滤**

NAT（网络地址转换）的行为由两个关键维度决定 \[[08:21](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=501)\]：

1.  **出站映射（Mapping）**：内网设备访问外网时，路由器如何分配公网端口。
    
    -   **端点无关映射（Endpoint Independent）**：只要内网端口相同，无论访问哪个外网目标，都映射到同一个公网端口。
        
    -   **端点相关映射（Dependent）**：访问不同的外网 IP 或端口时，会分配不同的公网端口。
        
2.  **入站过滤（Filtering）**：外网数据进入时，路由器是否放行。
    
    -   **端点无关过滤**：只要洞开了，任何人都能进来。
        
    -   **地址/端口相关过滤**：只有被内网主动访问过的 IP 或端口才能回传数据。
        

### **二、 四种主要的 NAT 类型**

根据 RFC3489 定义，NAT 可分为：

-   **NAT 1: Full Cone（全圆锥型）**：映射与过滤均为端点无关，最宽松，打洞最容易 \[[08:42](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=522)\]。
    
-   **NAT 2: Restricted Cone（受限圆锥型）**：映射无关，但过滤与 IP 相关 \[[08:52](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=532)\]。
    
-   **NAT 3: Port Restricted Cone（端口受限圆锥型）**：映射无关，过滤与 IP 和端口均相关 \[[09:03](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=543)\]。
    
-   **NAT 4: Symmetric（对称型）**：映射与过滤均与端点相关，最严格，通常难以直接打洞 \[[09:13](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=553)\]。
    

### **三、 P2P 打洞可行性实战演练**

视频通过 **Tailscale** 和 **Easytail** 演示了不同 NAT 组合下的连接情况：

-   **NAT 1 vs NAT 1/2/3**：非常容易实现 P2P 直连，延迟最低 \[[13:44](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=824)\]。
    
-   **NAT 3 vs NAT 3**：双方虽然都有端口限制，但通过互相发送“打洞”数据包建立关联状态，仍可实现直连 \[[17:10](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1030)\]。
    
-   **NAT 1 vs NAT 4**：由于 NAT 1 的宽松过滤，NAT 4 即使更换端口，NAT 1 也能接受并回传，可以打通 \[[19:04](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1144)\]。
    
-   **NAT 3 vs NAT 4**：常规 P2P 工具（如 Tailscale）通常无法打通，只能走中转 \[[20:13](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1213)\]。但 **Easytail** 可以利用“端口预测”和“批量打洞”技术，在几十秒内尝试碰撞出可用端口，实现直连 \[[22:17](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1337)\]。
    
-   **NAT 4 vs NAT 4**：打通难度极高且不稳定，行为类似网络攻击（疯狂占用 UDP 映射表），建议改善网络环境 \[[24:13](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1453)\]。
    

### **四、 关键应用与建议**

1.  **应用场景**：理解 NAT 类型对于**游戏联机**（减少延迟）、**BT 下载**（增加可连接性）和**远程访问家内设备**（无公网 IP 穿透）至关重要 \[[01:02](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=62)\]。
    
2.  **环境检测**：测试前需关闭所有代理工具。可以使用网页工具或 Easytail 客戶端检测 NAT 类型 \[[22:03](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1323)\]。
    
3.  **解决 NAT 4 问题**：
    
    -   尝试开启路由器的 **UPnP** 或 **DMZ** 区域 \[[25:25](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1525)\]。
        
    -   如果是运营商层面的大内网（CGNAT），用户端的操作可能无效，需联系运营商更改 NAT 类型 \[[26:18](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1578)\]。
        
    -   **IPv6**：如果双方都有 IPv6，通常可以绕过复杂的 IPv4 NAT 打洞问题 \[[26:32](http://www.youtube.com/watch?v=T8Fx9b8z5w0&t=1592)\]。
        

视频通过清晰的动画模拟了数据包在防火墙和 NAT 表中的流动过程，是理解 P2P 组网不可多得的教程。