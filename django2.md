#开发一个Django博客网站项目

####编写view.py视图函数

>视图函数或简称视图只是一个Python函数，它接受**Web请求并返回Web响应**。 此响应可以是网页的HTML内容，重定向，404错误，XML文档或图像。惯例是将视图放在名为views.py的文件中，放在项目或应用程序目录中。

我们在网站[Bootstrap](https://startbootstrap.com/)上直接找到免费的clean blog模板下载得到压缩包,解压后:

1. 先在django项目文件夹下新建static文件:模块django.contrib.staticfiles将各个应用的静态文件统一收集起来,这样所有的静态文件就会集中在一个便于发放的地方
2. 将压缩包中的js,css,vender,img都拖到这个文件夹下
3. 在setting.py中最后添加`STATICFILES_DIRS=[os.path.join('static')]`
4. 在所有的.html的文件中(templates中)加上`{% load static%}`,该模板标签会生成静态文件的绝对路径
5. 在所有的.html的文件中(templates中)所有的css,js,img的超链接都改为如下形式:以配合生成静态文件绝对路径
![23.png]()	
![25.png]()	
![26.png]()	
![30.png]()	

**下面开始编写视图函数**

![27.png]()	
![28.png]()	
![29.png]()	

**更改'post.html'和 'index.html'相应的地方为变量其他标题等地方也可以做出自己想要的修改**

**post.html**

![33.png]()	
![32.png]()	
![31.png]()	

**index.html**

![34.png]()


**注意:.html文件中使用到的变量都用{{}}括起来,而python语句都用{%%}括起来**

在app项目文件夹(blog)下建立一个templates文件夹将所需要使用的html文件都放进去,view.py所要用的的文件都会在这里寻找,这里的文件可以直接使用,不加路径.
![18.PNG](https://i.loli.net/2018/07/09/5b43827a1d445.png)

***

####运行django服务器


	`python manage.py runserver`  
![15.PNG](https://i.loli.net/2018/07/09/5b4366dbae98a.png)
显示上图即成功运行

####创建一个管理员的账号

`python manage.py createsuperuser`
![16.png](https://i.loli.net/2018/07/09/5b4367b838dae.png)
出现了success就可以了

###登陆
![17.PNG](https://i.loli.net/2018/07/09/5b43684638cc6.png)

