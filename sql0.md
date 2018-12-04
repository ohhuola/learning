######CTF中的SQL注入的思路总结

####1.拿到题目之后一般是一个登陆框(大概率盲注或者是报错,写脚本跑):

![1.png](https://i.loli.net/2018/09/11/5b979e5c0ee9b.png)

尝试输入用户名(admin)和密码(随意)查看其返回结果这里一般又分为两种:

1. 有返回是密码错误还是用户名错误
2. 只是单一的返回登陆失败

**对于第一种情况就可以直接尝试在`username`之后输入payload的了**

* 判断注入点
* 按照执行效果判断sql注入类型
* 利用字典跑一下被过滤的特殊符号和关键字(判断语句一般使用regexp或length(),and或者异或,再或者就是直接输入)
> payload `and length('关键字')>0`
		`^length('关键字')>0`
* 最后利用information_schema爆出库名,表名,字段名以及flag

**对于第二种情况就稍微要复杂一些了**

* 可以用burp抓个包,查看一下数据包中是否存在着一些提示内容
* 尝试一些万能的登陆密码:假设sql语句是:
`select * from user where username=’$username’ and password=’$password’ `
而我们我发送的数据包中是 `username= &password=`							


1. `username=reborn'='&password=reborn'='` => `select * from user where username='reborn'=' and password='reborn'='`=>`select * from user where 1 and 1`

2. `username=' or 1=1 %23&password=123456`

3. `username=admicsfn' union select '25d55ad283aa400af464c76d713c07ad'%23&password=12345678`利用不存在的用户再去构造一个密码来进行登陆

* 查看一下(利用字典)试一试哪一个特殊符号会报错之类的,如果有报错,则考虑报错注入
* 如果报错的是一些不常见的字符串,那可能就涉及了某些漏洞,比如%报错,就可能是sprintf格式化字符串漏洞
* 题外话:平时多浏览一些安全网站,去找新出现的漏洞,及时去做一些漏洞复现,与时俱进

####2.题目是GET注入(大概率是回显注入)

1. 判断注入点
2. 利用order by判断字段数
3. `union select 1,2,3,...%23`判断具体哪个字段存在回显
4. 利用字典跑一下被过滤的特殊符号和关键字(判断语句一般使用length(),and或者异或,再或者就是直接输入)
5. 最后利用information_schema爆出库名,表名,字段名以及flag
* 如果回显的内容一直不存在变化那就需要考虑盲注了

####3.页面直接返回注入结果(insert等其他类型的注入,考虑时间盲注)

可以使用`' and sleep(5)='1'='1`或`' and sleep(5)='1'='1' %23`来判断sleep()能否执行	
