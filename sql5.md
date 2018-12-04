######其他类型注入的详解(4)

###4.sql约束攻击

####知识点

1. 数据库字符串比较:在数据库对字符串进行比较时,如果两个字符的长度不一样,则会将较短的字符串末尾填充空格,使两个字符串的长度一致.

比如这两条语句
>`select * from admin where username='vampire'`</br>
	`select *from admin where username='vampire  '`

他们的查询结果是一致的.</br>

2. INSERT截断: 这是数据库的另一个特性,当设计一字段时,我们都必须对其设定一个最大长度,比如CHAR(255),VARCHAR(20),但是当长度超过限制的时候,数据库就会将其截断,只保留限定的长度.**(注意这里我们为什么需要insert注入:空格之后一定需要再跟一个或多个任意字符,防止程序在检查用户名是否已经存在时匹配到目标用户)**

####限制条件

1. 服务端没有对用户名长度进行限制.
2. 登陆验证的SQL语句没有和用户名和密码一起验证。如果是验证流程是先根据用户名查找出对应的密码，然后再比对密码的话，那么也不能进行利用。因为数据库一般取第一条则是目标用户的记录，那么你传输的密码肯定是和目标用户密码匹配不上的

####实战:Bugku-login1
[login1](http://123.206.31.85:49163/)