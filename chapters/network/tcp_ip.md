## TCP/IP协议

### TCP/IP模型和OSI模型

<table>
<tr>
    <td align="center"><b>OSI模型</b></td>
    <td align="center"><b>TCP/IP模型</b></td>
    <td align="center"><b>对应网络协议实例</b></td>
</tr>
<tr>
    <td align="center">应用层</td>
    <td align="center" rowspan="3">应用层</td>
    <td align="center">TFTP, FTP, NFS, WAIS</td>
</tr>
<tr>
    <td align="center">表示层</td>
    <td align="center">Telnet, Rlogin, SNMP, Gopher</td>
</tr>
<tr>
    <td align="center">会话层</td>
    <td align="center">SMTP, DNS</td>
</tr>
<tr>
    <td align="center">传输层</td>
    <td align="center">传输层</td>
    <td align="center">TCP, UDP</td>
</tr>
<tr>
    <td align="center">网络层</td>
    <td align="center">网际层</td>
    <td align="center">IP, ICMP, ARP, RARP, AKP, UUCP</td>
</tr>
<tr>
    <td align="center">数据链路层</td>
    <td align="center" rowspan="2">网络接口</td>
    <td align="center">FDDI, Ethernet, Arpanet, PDN, SLIP, PPP</td>
</tr>
<tr>
    <td align="center">物理层</td>
    <td align="center">IEEE 802.1A, IEEE 802.2到IEEE 802.11</td>
</tr>
</table>

### 三次握手


```
    Client                             Server
+-------------+                    +-------------+
|             |                    |LISTEN       |
|             |                    |(listen())   |
|     SYN_SENT| SYN seq=x          |             |
|   (connet())+-------------------->SYN_RCVD     |
|             |                    |   +         |
|             | SYN seq=y, ACK=x+1 |   |         |
|  ESTABLISHED<------------------------+         |
|       +      |                   |             |
|       |      | ACK=y+1           |             |
|       +-------------------------->ESTABLISHED  |
+-------------+                    +-------------+
```


### 四次挥手


```
    Client                             Server
+-------------+                    +-------------+
|   FIN_WAIT_1| SYN seq=x, ACK=y+1 |             |
|   ((close())+-------------------->CLOSE_WAIT   |
|             |                    |      +      |
|             |            ACK x+1 |      |      |
|   FIN_WAIT_2<---------------------------+      |
|             |                    |             |
|             |        FIN seq=y|1 |LAST_ACK     |
|   TIME_WAIT <--------------------+(close())    |
|       +     |                    |             |
|       |     | ACK y+1            |             |
|       +-------------------------->             |
+-------------+                    +-------------+
```


### TCP和UDP区别

|  | TCP | UDP |
| :---: | :---: | :---: |
| 是否连接 | 面向连接 | 面向非连接 |
| 传输可靠性 | 应用层 | 不可靠的 |
| 应用场合 | 传输大量的数据 | 传输少量数据 |

TCP协议和UDP协议特性区别：

1. TCP协议在传送数据段的时候要给段标号；UDP协议不需要
2. TCP协议是可靠的；UDP协议是不可靠的
3. TCP协议是面向连接；UDP协议采用无连接
4. TCP协议负载较高，采用虚拟电路方式；UDP采用无连接
   方式
5. TCP协议的发送方要确认接收方是否收到数据段（3次握手协议）
6. TCP协议采用窗口技术和流控制
