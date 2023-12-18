---
title: python面向对象
date: 2023-12-18 23:28:24
tags: python
categories: python
---

### 基本概念

------

* **类**：一群有着相同属性和函数(方法)的对象(实例)的集合，也可以具象化的理解为是一群有着相似特征的事物的集合；用class来声明。

  抽象类：是一种特殊的类，只能作为父类存在，一旦对象化（或叫实例化）就会报错；一般使用`class Classname(metaclass=ABCMeta)`来声明。

* **类的继承**：子类继承父类，子类可以使用父类的属性和函数，同时子类可以有自己独特的属性和函数；子类在生成对象的时候（实例化时），是不会自动调用父类的构造函数的，必须在子类的构造函数中显示的调用父类的构造函数；继承的优势是减少重复代码，降低系统熵值（即复杂度）。

* **属性**：用"`self.属性名`"来表示，通过构造函数传入；表示对象(实例)的某个静态特征。

  1. 私有属性：以__开头的属性，举例：`self.__`属性名，只能在类内部调用，类外部无法访问。

  2. 公有属性：和函数并列声明的属性，可以理解为常量，一般用全大写表示；在类中通过"`self.常量名`"来调用，在类外使用"对象名.常量名"或者"类名.常量名"来调用。

* **函数**：表示对象(实例)的某个动态能力。

  1. 构造函数：用`def __init__（self, args...）`声明，第一个参数self代表当前对象的引用，其他参数是在对象化时需要传入的属性值；构造函数在一个对象生成时(即实例化时)会被自动调用。
  1. 成员函数：是正常的类的函数，第一个参数必须是self；可通过此函数来实现查询或修改类的属性等功能。
  1. 静态函数：属于当前类的命名空间下，且对第一个参数没有要求；一般用来做一些简单独立的任务，既方便测试也能优化代码结构；一般使用装饰器`@staticmethod`来声明。
  1. 抽象函数：一般定义在抽象类中，主要目的是要求子类必须重载该函数才能正常使用；使用装饰器`@abstractmethod`来声明。
  1. 函数重载：父类的某函数通过`raise Exception`的方式要求子类必须重写该函数来覆盖父类原有函数。

<font color="red">注意：<br>每个类都有构造函数，继承类在生成对象的时候，是不会自动调用父类的构造函数的，因此你必须在 `init()` 函数中显式调用父类的构造函数。它们的执行顺序是 子类的构造函数 -> 父类的构造函数。</font>

```python
例如：Document类继承了Entity类，Entity.__init__(self, 'document')属于显式调用父类的构造函数
class Document(Entity): 
    def __init__(self, title, author, context):
        print('Document class init called') 
        Entity.__init__(self, 'document') 
        self.title = title 
        self.author = author 
        self.__context = context
```

* **对象**：类对象化(实例化)后的某一个具体事物。

* Python并没有真正的**私有化**支持，但可用下划线得到伪私有：

  （1）`_xxx` "单下划线 " 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量，需通过类提供的接口进行访问；

  （2）`__xxx` 类中的私有变量/方法名，只有类对象自己能访问，连子类对象也不能访问到这个数据。

  （3）`__xxx__` 魔法函数，前后均有一个“双下划线” 代表python里特殊方法专用的标识，如 `__init__()` 代表类的构造函数。

Q: 如何继承父类的构造函数?

A: Python 3 中继承写法 `super().__init__()`

​    Python 2 中继承写法 `super(子类名, self).__init__()`，且在父类定义时要在括号内指定 object。



### 常用的魔术方法

------

* `__new__`：

​	创建对象并分配内存，应用中可改变对象的创建过程。比如，初始化之前，检查缓存中是否存在该	对象，如果存在则将缓存存放对象直接返回，如果不存在，则将对象放至缓存中，供下次使用

* `__init__`

  构造方法，初始化对象

* `__del__`

  析构方法，调用del方法，销毁对象，释放空间

  Python 采用自动引用计数（简称 ARC）的方式实现垃圾回收机制。该方法的核心思想是：每个 Python 对象都会配置一个计数器，初始 Python 实例对象的计数器值都为 0，如果有变量引用该实例对象，其计数器的值会加 1，依次类推；反之，每当一个变量取消对该实例对象的引用，计数器会减 1。如果一个 Python 对象的的计数器值为 0，则表明没有变量引用该 Python 对象，即证明程序不再需要它，此时 Python 就会自动调用 `__del__()` 方法将其回收。

* `__str__`

  自定义对象的打印格式，原本打印实例化对象显示内存地址，现在显示成自定义的字符串

  ```python
  class Dog:
      def __init__(self, name):
          self.name = name
          print("The dog is %s" %self.name)
          def __str__(self):
          return "Dogs"
  
  husky = Dog("husky")
  print(husky)
  ```

* `__call__`

  类实例化的对象变得可调用，callable

  当对一个对象加()进行调用的时候，__call__方法会自动执行，可以用于类装饰器

  ```python
  class CalTime(object):
      def __init__(self,func):#返回一个对象：__init__方法首先会初始化对象，并把对象返回。
          self.func=func
          print("~~~~~~")
  
      def __call__(self,*args,**kwargs):#__call__当对一个对象加()进行调用的时候，此方法会自动执行
          print(f"{self.func.__name__}函数运行前")
          start=time.time()
          result=self.func(*args,**kwargs)
          print(f"{self.func.__name__}函数运行后")
          end=time.time()
          self.log()
          print(f'{self.func.__name__}的运行时间为{end-start}')
          returnresult
  
      def log(self):
          pass
      
  @CalTime #类，f会传给类，然会返回一个A的实例
  def f(x,y):
      time.sleep(1)
      print(x)
      pass
      return x+y
  ```

* `__add__`

  两个同类型的对象执行加操作

  ......
