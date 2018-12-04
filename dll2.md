<h1>DLL注入(二):注入实战</h1>

<h3>1.DLL注入的定义</h3>
DLL注入是将代码插入正在运行的进程的过程。我们通常插入的代码是动态链接库（DLL）的形式，因为DLL意味着在运行时根据需要加载。要注意的是:需要在系统上拥有适当级别的权限才能开始播放其他程序的内存.
<hr>
<h3>2.DLL注入的主要步骤</h3>
1. 附加到流程
2. 在流程中分配内存
3. 将DLL到进程内存中并确定适当的内存地址
4. 指示进程执行DLL(我们在这里采用的是CreateRemoteThread函数)

![2.png](https://i.loli.net/2018/10/02/5bb36c492f0bb.png)

<hr>
<h4>2.1.附加到流程</h4>
首先，我们需要一个流程句柄，以便我们可以与它进行交互。这是通过
[OpenProcess()](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-openprocess)
函数完成的。我们还需要请求某些访问权限，以便我们执行以下任务。我们请求的特定访问权限因Windows版本而异，但以下内容适用于大多数：

![3.png](https://i.loli.net/2018/10/02/5bb37ec1a0631.png)

<h4>2.2.分配内存</h4>
在我们将任何东西注入另一个过程之前，我们需要一个放置它的地方。我们将使用该VirtualAllocEx()功能来执行此操作。
<a href='https://msdn.microsoft.com/en-us/library/windows/desktop/aa366890(v=vs.85).aspx'>VirtualAllocEx()</a>
占用内存量作为其参数之一。如果我们使用LoadLibraryA()，我们将为DLL的完整路径分配空间.

![4.png](https://i.loli.net/2018/10/02/5bb37ec207ce3.png)

<h4>2.3.将DLL到进程内存中并确定适当的内存地址</h4>
现在我们在目标进程中分配了空间，我们可以将DLL Path复制到该进程中。我们将使用
<a href='https://msdn.microsoft.com/en-us/library/windows/desktop/ms681674(v=vs.85).aspx'>WriteProcessMemory()</a>
来执行此操作:

![5.png](https://i.loli.net/2018/10/02/5bb37ec28cc0c.png)

<h4>2.4.指示进程执行DLL</h4>

[CreateThread()](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createthread)

![6.png](https://i.loli.net/2018/10/02/5bb37ec293b91.png)

<h4>2.5.其他的重要函数</h4>
<a href='https://msdn.microsoft.com/en-us/library/windows/desktop/ms684175(v=vs.85).aspx'>LoadLibraryA()</a>:将指定的模块加载到调用进程的地址空间中。指定的模块可能会导致加载其他模块

<a href='https://docs.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobject'>WaitForSingleObject()</a>:等待直到指定的对象处于信号状态或超时间隔过去。