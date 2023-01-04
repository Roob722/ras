##   记录一次模拟登录与RAS解密过程
#### 这是一个小众使用的抢单网站，登录比较简单，只需要传递验证码就可以。
- 主页长这样![主页长这样](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/1.png)
- 登录只需要传递用户名密码跟验证码![登录只需要传递用户名密码跟验证码](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/2.png)
- 先写一个自动登录函数![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/3.png)
- 网址对返回的数据包加密了![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/4.png)
- 发起订单请求看样子只需要订单的Id!,没找到订单id的明文，估计是在加密的数据包里[](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/5.png)
- 找到解密数据包的js函数。在标记处打上断点，确定就是这个函数解密的![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/6.png)
- 大概看了一下这个加解密js代码，如果用python还原有点难度，直接拿下整个js文件![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/7.png)
- 要调用这份js文件需要在文件开头加上这3个，不然会提示各种Undefined![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/8.png)
- 还需要在js文件里加上括起来这句![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/9.png)
- 把解密调用的函数放在开头 ![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/10.png)
- 这里有个坑，execjs 默认是用gbk，需要把他改成utf-8![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/11.png)
![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/12.png)
- 写个函数调用js，这里就成功解密返回的数据包了,id果然在加密数据包里面![](http://yungengxin.oss-cn-beijing.aliyuncs.com/%E7%A0%81%E5%95%86/%E6%8A%A2%E8%B4%AD/13.png)
- 剩下抢单就判断很简单了，就不贴上代码了！
