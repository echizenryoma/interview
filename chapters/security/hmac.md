## 消息认证码

### HMAC

#### 算法

![](https://upload.wikimedia.org/wikipedia/commons/7/7f/SHAhmac.svg)

根据RFC 2104，HMAC的数学公式为： $$ {\displaystyle {\textit {HMAC}}(K,m)=H{\Bigl (}(K'\oplus opad)\;||\;H{\bigl (}(K'\oplus ipad)\;||\;m{\bigr )}{\Bigr )}} $$

其中：

$$H$$为密码散列函数（如MD5或SHA-1）

$$K$$为密钥（secret key）

$$m$$是要认证的消息

$$K'$$是从原始密钥$$K$$导出的另一个秘密密钥（如果K短于散列函数的输入块大小，则向右填充零；如果比该块大小更长，则对K进行散列）

$$||$$代表串接

$$\oplus$$代表异或（XOR）

$$opad$$是外部填充（0x5c5c5c…5c5c，一段十六进制常量）

$$ipad$$是内部填充（0x363636…3636，一段十六进制常量）

