* sign生成规则及步骤：
    * 第一步：将所有需要发送至服务端的请求参数（appkey、sign除外）按照参数名ASCII码从小到大排序（字典序）

        注意：如果参数的值为空不参与签名；

    * 第二步：将排序后的参数按照URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串strA；

    * 第三步：在strA后面拼接上appKey和salt得到striSignTemp字符串，将strSignTemp字符串转换为小写字符串后进行MD5运算，MD5运算后得到值作为sign的值传入服务端；

* 拒绝重复调用
```
    客户端第一次访问时，将签名sign存放到缓存服务器中，超时时间设定为跟时间戳的超时时间一致，二者时间一致可以保证无论在timestamp限定时间内还是外 URL都只能访问一次。
    如果有人使用同一个URL再次访问，如果发现缓存服务器中已经存在了本次签名，则拒绝服务。如果在缓存中的签名失效的情况下，有人使用同一个URL再次访问，则会被时间戳超时机制拦截。
    这就是为什么要求时间戳的超时时间要设定为跟时间戳的超时时间一致。拒绝重复调用机制确保URL被别人截获了也无法使用（如抓取数据）
```