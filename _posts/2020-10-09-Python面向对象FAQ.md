---
layout: post
title: Python面向对象FAQ
date: 2020-10-09
categories:
- python
tags:
- oop
---
## 面向对象Q&A

### 1. 问: 为什么类的私有属性(例如:__name),从外部不能直接访问?

答: 因为实例化以后会改变私有属性的名称, 变成 "\_\_类名\_\_私有属性", 所以不能用原来的私有属性直接访问;另外也并不建议使用"\_\_类名\_\_私有属性"去强行访问,因为不同版本的解释器有可能会把私有属性改成不同的属性名!

### 2. 问: 一个类中的单下划线属性(例如: _age)是私有属性吗?

答: 其实单下划线开头的属性并不是私有属性,但是约定俗称的视为私有, 所以即使可以从外部直接访问,但是不建议

### 3. 问: 类里面前后都有双下划线的属性(例如:\_\_name\_\_)是啥,能直接访问吗?

答: 是特殊属性, 可以直接从外部访问, 但是因为本身有特殊用途的属性才这样命名,所以一般我们定义的属性不能使用这种方式命名!

### 4. 问: 私有属性不能从外部访问, 那如果就想访问怎么办呢?

答: 可以定义获取私有属性的方法, 把私有属性给return出来

```python
class Student(object):
	def __init__(self, name):
        self.__name = name
    def get_name(self):
        return self.__name


stu = Student("laowang")
stu.get_name()  # 调用方法获取私有属性
```

### 5. 问: 不仅想从外部访问私有属性, 还想修改私有属性,怎么办?

答: 还是可以定义一个修改私有属性的方法,通过调用方法去修改私有属性的值

```python
def set_name(self, name):
        self.__name = name
        
stu.set_name("xiaowang")
```

### 6. 问 : 如果从外部动态的修改私有属性,能修改成功吗?

答: 首先从外部动态绑定的属性, 即使前缀是双下划线, 甚至和原来的私有属性同名, 它都不会是一个私有属性, 而是一个新的普通实例属性, 可以从外部直接访问;而且私有属性在实例化以后其实已经不再是原来的名字了(新的名字不容易推断,会根据解释器不同而改变), 所以仅和类中定义的私有属性同名是无法修改私有属性的

### 7. 问: 私有属性干什么用的?

答: 限制访问用的, 当你不想让外部代码随意改变对象的内部状态时, 就可以使用私有属性(私有变量)把内部变量保护起来

### 8. 问: 啥是基类?啥是父类?啥又是超类?

答: 基类 父类 超类 本身就是同一个概念, 都是指面向对象继承中被继承的一方!

### 9: 问: 多态是啥东西?

答: 我们在定义类class时, 其实就是定义了一个数据类型, 那么实例后的对象自然就属于这个类型; 又因为有继承的存在, 那么实例对象既是当前类的数据类型,又是父类的数据类型, 反之则不行!

```python
class Animal(object):
    def is_animal(self):
        print("is animal")


class Dog(Animal):
    def is_dog(self):
        print("is dog")


xb = Dog()
print(isinstance(xb, Dog))  # True
print(isinstance(xb, Animal))  # True 是狗一定是动物

dh = Animal()
print(isinstance(dh, Animal))  # True
print(isinstance(dh, Dog))  # False 是动物但不一定是狗
```

### 10. 问: 多态有啥好处?

答: 继承自统一个父类的所有子类, 如果都有一个同名方法, 那么如果公共方法正好调用对象的这个同名方法时, 在传入不同实例的情况下会自动调用对应的方法, 而不用修改公共方法本身!

```python
def run_once(animal):
    animal.run()
    

class Animal(object):
    def run(self):
        print("Animal is running...")
        

class Dog(Animal):
    def run(self):
        print("Dog is running...")
        

class Cat(Animal):
    def run(self):
        print("Cat is running...")

     
run_once(Animal())  # "Animal is running..."
run_once(Dog())  # "Dog is running..."
run_once(Cat())  # "Cat is running..."
```

### 11. 问: 啥是鸭子类型?

答:  对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。对于Python这样的动态语言来说，则不一定需要传入`Animal`类型。我们只需要保证传入的对象有一个`run()`方法就可以了;这就是动态语言的“鸭子类型”，它并不要求严格的继承体系

```python
class Duck(object):  # Duck继承自object类,并没继承自Animal类
    def run(self):
        print("Duck is running...")
        
run_once(Duck())  # "Duck is running..."  既使不继承自Animal依然可以调用     
```

### 12. 问: 可以给类动态地绑定方法吗?

答: 可以, 直接在外部使用 "类名.方法名 = 方法"即可

### 13. 问: 想要限制从外部添加的实例属性, 怎么做?

答:在类的内部, 使用特殊变量\_\_slots\_\_=(" ", "  ", ...)去限定能够添加的属性名, 试图从外部添加限定外的属性时, 会报错

### 14. 问: \_\_slots\_\_定义的限制属性对子类实例起作用吗?

答: \_\_slots\_\_定义的属性仅对当前类实例起作用，对继承的子类是不起作用的;

### 15. 问: 如果父类定义了 \_\_slots\_\_, 子类中又定义了 \_\_slots\_\_会怎么样?

答: 此时, 子类实例限制添加的属性就等于"父类限制+子类限制", 也就是如果子类也定义 \_\_slots\_\_则自动继承父类的 \_\_slots\_\_

### 16. 问: 为什么需要@property装饰器呢?

答: 直接使用外部添加、修改、访问的方式去操作属性, 太暴露且不能定义参数检验, 容易导致属性被胡乱修改; 而在类的内部对一个属性定义多个操作函数又略显杂乱, 所以此时就需要@property装饰器把函数当属性用

### 17. 问 : @property装饰器的好处是啥?

答: 可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性

### 18. 问: 怎么使用@property装饰器呢?

答: 首先在获取属性的方法加上@property装饰器, 然后在设置(修改)属性的方法上面加上@属性名.setter的装饰器, 如果是只读属性就在读方法上面加一个@property装饰器即可

```python
class Student(object):

    @property
    def birth(self):  # birth的读方法
        return self._birth

    @birth.setter
    def birth(self, value):  # birth的写方法
        self._birth = value

    @property
    def age(self):  # 只读属性
        return 2015 - self._birth
```

### 19. 问: 怎么理解Mixin?

答: 多继承中,非主线继承的类, 为了增加混入的其他功能的类在命名时一般会加上Mixin以作区别;通过直接继承Mixin类,可以避免设计过于复杂的继承路线, 使类的层级结构更清晰

### 20. 问: \_\_str\_\_和\_\_repr\_\_分别是干嘛用的?

答: 都是可以自定义输出格式的方法; \_\_str\_\_主要面向用户, 而 \_\_repr\_\_主要是给程序员看的, 使用print()方法打印对象, 才会调用 \_\_str\_\_, 如果直接显示变量(例如在控制台)则会调用 \_\_repr\_\_; 偷懒的定义方法, 只定义一个\_\_str\_\_,然后 \_\_repr\_\_ = \_\_str\_\_即可统一输出格式

### 21. 问: \_\_iter\_\_()干嘛用的?\_\_next\_\_()又是干嘛用的?

答: 如果一个类想要用于for in 循环, 则必须实现一个\_\_iter\_\_()方法, 返回一个可迭代对象, 也就是实例本身self ; for循环就会不断地调用实例的\_\_next\_\_()方法拿到循环的下一个值, 直到StopIteration

### 22. 问: 一个可迭代的实例对象, 想要使用下标和切片访问, 怎么实现?

答: 使用\_\_getitem\_\_()方法实现, 注意需要判断传入的参数是int还是slice类型, 以区别下标访问和切片访问; 但是这里的切片不能对step参数处理, 也就是不能自定义步长, 同时也不能使用复数

### 23. 问: 除了\_\_getitem\_\_()以外, \_\_setitem\_\_()和\_\_delitem\_\_()又是干嘛用的?

答:  \_\_setitem\_\_()把实例对象视为list或dict来对集合赋值, \_\_delitem\_\_()用来删除某个元素; 通过这3个方法, 我们自己构造的类和python自带的list, tuple, dict 也差不多了, 类似鸭子类型

### 24. 问: \_\_getattr\_\_(self, attr)是用来做什么的?

答: 访问类中不存在的属性或者调用不存在的方法时, 会调用 \_\_getattr\_\_方法, 可以自定义返回的值 ; 默认返回None值, 可以使用 raise AttributeError ("  ", attr)去格式化想输出的错误信息

### 25. 问: 当一个实例想要调用实例方法时, 一般会以instance.method()这种方式调用, 那可不可以直接使用instance()调用呢?

答: 可以, 定义一个\_\_call\_\_()方法, 相当于就把实例对象看成一个函数去调用

### 26. 问: 那怎么判断一个对象能否被调用呢?

答: 只需要判断一个对象是否可以被调用, 使用callable(对象)函数判断是否是可调用callable的对象, 

### 27. 问: 需要定义常量时, 怎么使用枚举类?

答: from enum import Enum 导入枚举类, 枚举类的value属性是自动赋值给成员的int型常量, 注意是从1开始的   ; 还可以导入@unique装饰器保证没有重复值

### 28. 问: 不使用class还能怎么样创建一个类呢?

答: 使用type()既可以查看对象的类型, 同时又可用于创建类 ; 事实上解释器在扫描完class定义的语法后就是使用type去创建这个类的; 使用type()相当于是动态的创建类, type(类名, (继承的父类, ...), dict(类的方法名=外部方法)) 

### 29. 问: 什么是元类metaclass?

答:  元类允许创建或修改类 metaclass\=\=>class\=\=>instance, 元类用来控制类的创建行为, 定义元类一般以Metaclass结尾, 是Python面向对象里最难理解，也是最难使用的魔术代码
