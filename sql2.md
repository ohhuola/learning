######其他类型注入的详解(1)

###1.二次注入
**形成原理:在第一次进入数据库插入数据的时候,仅仅只是使用了addslashes或者是借助get_magic_quotes_gpc对其中的特殊字符进行转义,但是addslashes有一个特点就是虽然在参数前会添加'\'进行转义,但是'\'并不会进入数据库中,再写入数据库时还是保留了原来输入的数据.在下一次需要查询时就直接从数据库中取出了能够构成payload的语句.比如在第一次注入时写入了单引号,就可以形成二次注入**

*Tips:*一般题目中都会存在**登陆注册**两种功能(当然也不排除session文件包含)

####例子:(sqli-labs less-24)
1. 首先进入搭建好的sqli-labs less-24
2. 注册一个新的账号`admin'#`

![1.png](https://i.loli.net/2018/09/11/5b97890e69b76.png)

3. 查看此时数据库中已经多出一个账号了并且特殊符号没有被转义

![2.png](https://i.loli.net/2018/09/11/5b97890e9a1eb.png)

4. 我们尝试着登陆进去,会发现可以更改当前用户的密码

![3.png](https://i.loli.net/2018/09/11/5b97890ea5883.png)

5. 到数据库查看可以发现admin的密码被修改了,而admin'#的密码没有

![4.png](https://i.loli.net/2018/09/11/5b97890ec53e5.png)

6. 我们可以查看pass_change.php的代码,发现修改密码这里的sql语句
"UPDATE users SET PASSWORD='$pass' where username='$username' and password='$curr_pass' ";
![5.png](https://i.loli.net/2018/09/11/5b978dbab7453.png)

我们可以看到如果将`admin'#`带到里面去后:
`"UPDATE users SET PASSWORD='$pass' where username='admin'# and password='$curr_pass' ";`
实际上就是:
`"UPDATE users SET PASSWORD='$pass' where username='admin'`
这里后面的内容都被#注释掉了,这样就是在修改admin的密码了,我们再尝试登陆一下,发现利用admin也可以成功了

![6.png](https://i.loli.net/2018/09/11/5b9791c91357c.png)

