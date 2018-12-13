---
title: Python入门
date: 2018-12-11 19:54:53
categories: 后端
tags: [Python, 深度学习]
---

# Python是什么 #
>  Python是一个简单、易读、易记的编程语言，而且而且是开源的，可以免费地自由使用。Python可以用类似英语的语法编写程序，编译起来也不费力，因此我们可以很轻松的使用Python。
>  
> 此外，使用Python不仅可以写出可读性高的代码，还可以写出性能高的代码，在需要处理大规模数据或者要求快速强硬的情况下，使用Python可以稳妥的完成。
> 
>  再者，在科学领域，特别是机器学习、数据科学领域，Python也被大量使用。Python除了高性能之外，凭借NumPy、SciPy等优秀的数值计算、统计分析库，在数据科学领域再有不可动摇的地位。Caffe、TensorFlow、Chainer、Theano等深度学习框架也都提供了Python接口，因此，学习Python对使用深度学习框架大有益处。
>  
> 综上，Python是最适合数据科学领域的编程语言，也是初学者学习深度学习最合适的工具。

# Python安装 #
> Python有Python2.x和Python3.x两个版本，两个版本都被大量的使用但不相兼容，安装时应当注意。
> 
> 为了有效的促进深度学习的实现，需要安装并学习NumPy库和Matplotlib库的使用。NumPy是用于数值计算的库，提供了很多高级的数学算法和便利的数组(矩阵)操作方法。Matplotlib是用来画图的库，能将实验结果可视化。
> 
> Python的安装方法有很多种，这里推荐Anaconda Python3.x发行版。Anaconda是一个侧重于数据分析的发行版，其中集成NumPy，Matplotlib等有助于数据分析的库。

# Python解释器 #
> 完成Python的安装后，要确认Python的版本。打开cmd，输入Python --version。

    python --version
	Python 3.7.0

> 确认安装版本后，启动Python解释器。输入python。

    python
	Python 3.7.0 (default, Jun 28 2018, 08:04:48) [MSC v.1912 64 bit (AMD64)] :: Anaconda, Inc. on win32
	Type "help", "copyright", "credits" or "license" for more information.
	>>>
> Python解释器也被称为“对话模式”，用户能够以Python对话的方式进行编程。以下借助对话介绍Python基本语法。

## 1. 算数计算 ##
> *表示乘法，/ 表示除法，** 表示乘方，注意整数除以整数，在Python2.x中为整数，Python3.x中为浮点数。
 
	>>> 1 + 2
	3
	>>> 1 - 2
	-1
	>>> 4 * 5
	20
	>>> 7 / 5
	1.4
	>>> 3 ** 2
	9

## 2. 数据类型 ##
> 数据类型表示数据的性质，有整数、小数、字符串等类型，type()可以用来查看数据类型。
 
	>>> type(10)
	<class 'int'>
	>>> type(2.718)
	<class 'float'>
	>>> type("hello")
	<class 'str'>

## 3. 变量 ##
> Python是属于“动态类型语言”的编程语言，所谓动态，是指变量的类型是根据情况自动决定的。“#”为注释

	>>> x = 10      # 初始化
	>>> print(x)    # 输出x
	10
	>>> x = 100     # 赋值
	>>> print(x)
	100
	>>> y = 3.14
	>>> x * y
	314.0
	>>> type(x * y)
	<class 'float'>

## 4. 列表 ##
> 列表(数组)用于汇总数据，可以通过索引(下标)访问元素。

	>>> a = [1, 2, 3, 4, 5]  # 生成列表  
	>>> print(a)    # 输出列表内容   
	[1, 2, 3, 4, 5]
	>>> len(a)      # 获取列表长度
	5
	>>> a[0]        # 访问第一个元素的值
	1
	>>> a[4]
	5
	>>> a[4] = 99   # 赋值
	>>> print(a)
	[1, 2, 3, 4, 99]
> 列表还提供了切片(slicing)标记法，切片不仅可以访问某个值，还可以访问子列表。

	>>> print(a)    
	[1, 2, 3, 4, 99]
	>>> a[0:2]      # 获取索引为0到2（不包括2！）的元素      
	[1, 2]
	>>> a[1:]       # 获取从索引为1的元素到最后一个元素
	[2, 3, 4, 99]
	>>> a[:3]       # 获取从第一个元素到索引为3（不包括3！）的元素
	[1, 2, 3]
	>>> a[:-1]      # 获取从第一个元素到最后一个元素的前一个元素之间的元素
	[1, 2, 3, 4]
	>>> a[:-2]      # 获取从第一个元素到最后一个元素的前二个元素之间的元素
	[1, 2, 3]

## 5. 字典 ##
> 列表以索引存储数据，字典以键值对形式存储数据。

	>>> me = {'height':180}  # 生成字典
	>>> me['height']         # 访问字典
	180
	>>> me['weight'] = 70    # 添加新字典
	>>> print(me)
	{'height': 180, 'weight': 70}

## 6. 布尔型 ##
> bool型取True或False中的一个值，针对bool型运算符包括and、or、not。

	>>> hungry = True     # 饿了
	>>> sleepy = False    # 困了
	>>> type(hungry)
	<class 'bool'>
	>>> not hungry
	False
	>>> hungry and sleepy # 饿并且困
	False
	>>> hungry or sleepy  # 饿或者困
	True

## 7. if语句 ##
> 根据不同的条件选择不同的处理分支可使用if/else语句。使用时注意缩进，推荐使用4个空白字符。

	>>> hungry = True
	>>> if hungry:
	...     print("I'm hungry")
	...
	I'm hungry
	>>> hungry = False
	>>> if hungry:
	...     print("I'm hungry")
	... else:
	...     print("I'm not hungry")
	...     print("I'm sleepy")
	...
	I'm not hungry
	I'm sleepy

## 8. for语句 ##
> 进行循环处理可使用for语句

	>>> for i in [1, 2, 3]:
	...     print(i)
	...
	1
	2
    3

## 9. 函数 ##
> 将一连串的处理定义成函数(function)。字符串拼接可以用+。

	>>> def hello():
	...     print("hello world!")
	...
	>>> hello()
	hello world!
	
	>>> def hello(object):
	...     print("Hello " + object + "!")
	...
	>>> hello("cat")
	Hello cat!

## 10. 类 ##
> 之前int和str为系统内置的数据类型，我们可以自己定义新的类，并定义方法与属性。

	class 类名：
	def __init__(self, 参数, …): # 构造函数
	...
	def 方法名1(self, 参数, …): # 方法1
	...
	def 方法名2(self, 参数, …): # 方法2
	...
> \__init__为初始化方法，也成为构造方法，此外，Python第一个参数需要以self表明自身。

	class Man:
		def __init__(self, name):
			self.name = name
			print("Initialized!")
	
		def hello(self):
			print("Hello " + self.name + "!")
	
		def goodbye(self):
			print("Good-bye " +self.name + "!")
	
	m = Man("David")
	m.hello()
	m.goodbye()

	Initialized!
	Hello David!
	Good-bye David!
	
# Python脚本文件 #
> Python解释器能够以对话模式执行程序，非常便于进行简单的实验。但是进行一连串的处理并不方便，这时可将Python程序保存为文件，运行这个文件即可。这就是Python脚本文件。
> 
> 打开文本编辑器，新建一个hungry.py文件，包含一条一句语句

    print("I'm hungry")
> 打开cmd终端，移动到文件目录下，执行python hungry.py命令

    python hungry.py
	I'm hungry
# NumPy #
> NumPy是Python的外部库，需要导入方可使用。

	import numpy as np  # 将numpy作为np导入
> 深度学习中将经常用到NumPy数组类的操作。

	>>> x = np.array([1.0, 2.0, 3.0])
	>>> print(x)
	[1. 2. 3.]
	>>> type(x)
	<class 'numpy.ndarray'>
> NumPy数组之间可以进行算数运算。

	>>> x = np.array([1.0, 2.0, 3.0])
	>>> y = np.array([2.0, 4.0, 6.0])
	>>> x + y  # 对应元素加法
	array([3., 6., 9.])
	>>> x - y
	array([-1., -2., -3.])
	>>> x * y  # 对应元素乘法
	array([ 2.,  8., 18.])
	>>> x / y
	array([0.5, 0.5, 0.5])
> NumPy可以实现多维数组，并执行多维数组之间的操作

	>>> A = np.array([[1, 2], [3, 4]])
	>>> print(A)
	[[1 2]
	 [3 4]]
	>>> A.shape  # 查看数组形状
	(2, 2)
	>>> A.dtype  # 查看数组数据类型
	dtype('int32')
	>>> B = np.array([[3, 0], [0, 6]])
	>>> A + B    # 对应元素加法
	array([[ 4,  2],
	       [ 3, 10]])
	>>> A * B    # 对应元素乘法
	array([[ 3,  0],
	       [ 0, 24]])
> 在NumPy中，形状不同的数组之间也可以进行运算，称之为广播。

	>>> A = np.array([[1, 2], [3, 4]])
	>>> A * 10
	array([[10, 20],
	       [30, 40]])
	>>> B = np.array([10, 20])
	>>> A * B
	array([[10, 40],
	       [30, 80]])
![png2](/picture/2018-12-13-02.png)
![png1](/picture/2018-12-13-01.png)
> 访问NumPy数组的元素需要通过索引、for循环或者标记法。

	>>> X = np.array([[51, 55], [14, 19], [0, 4]])
	>>> print(X)
	[[51 55]
	 [14 19]
	 [ 0  4]]
	>>> X[0]
	array([51, 55])
	>>> X[0][1]
	55
	
	>>> for row in X:
	...     print(row)
	...
	[51 55]
	[14 19]
	[0 4]
	
	>>> X > 15
	array([[ True,  True],
	       [False,  True],
	       [False, False]])
	>>> X[X>15]
	array([51, 55, 19])	

# Matplotlib #
> Matplotlib是Python的外部库，可以轻松绘制图形和实现数据的可视化。
> 
> 使用matplotlib的pyplot绘制sin函数曲线

	import numpy as np 
	import matplotlib.pyplot as plt 
	
	# 生成数据
	x = np.arange(0, 6, 0.1) # 以0.1为单位，生成0到6的数据
	y = np.sin(x);
	
	# 绘制图形
	plt.plot(x, y);
	plt.show()
![sin函数](/picture/2018-12-13-03.png)
> 通过pyplot实现追加cos函数、添加标题、x轴签名等功能。

	import numpy as np 
	import matplotlib.pyplot as plt 
	
	# 生成数据
	x = np.arange(0, 6, 0.1) # 以0.1为单位，生成0到6的数据
	y1 = np.sin(x)
	y2 = np.cos(x)
	
	# 绘制图形
	plt.plot(x, y1, label="sin")
	plt.plot(x, y2, linestyle="--", label="cos") # 用虚线绘制
	plt.xlabel("x") # x轴标签
	plt.ylabel("y") # y轴标签
	plt.title("sin & cos") # 标题
	plt.legend()
	plt.show()
![sin函数和cos函数](/picture/2018-12-13-04.png)
> pyplot中还提供了imshow()用于显示图像，image中imread()用于读入图像。

	import matplotlib.pyplot as plt 
	from matplotlib.image import imread
	
	img = imread("../img/3.jpg"); # 读入图像(设置合适的路径！)
	plt.imshow(img)
	
	plt.show()
![显示图像](/picture/2018-12-13-05.png)




	
