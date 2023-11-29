---
hide:
  - tags
tags:
  - 单例模式
  - 工厂模式
  - 工厂方法模式
---
# Python

## 单例模式

单例模式：确保一个类只有一个实例。
如：服务器的配置信息存在在一个文件中，客户端通过AppConfig类读取配置信息。在程序运行过程中，很多地方都会用到配置文件信息，就需要创建很多AppConfig实例。
这导致内存中有很多AppConfig对象的实例，造成资源的浪费，只希望Appconfig只有一份，需要使用单例模式。

### 实现单例模式方法

#### 1、使用模块

其实python模块就是天然的单例模式，模块第一次导入的时候，会生成.pyc文件，当第二次导入的时候，就会直接加载.pyc文件，如果把相关的函数和数据定义在模块中，就可以获得一个单例对象

新建一个python模块singleton，通过mysingleton.py

```py

class Singleton:
    def foo(self):
        pass
singleton = Singleton()
```
使用
```py
# 实例化了一个对象singleton，直接导入这个对象，而不是在实例化一个，这样就实现了单例模式。

# 这种模块实现单例的方法，无法传递参数
from mysingleton import singleton
```
#### 2、使用装饰器
给类定义一个装饰器函数，在装饰器中的外层变量定义一个字典，里面存放这个类的实例。
当第一次创建时，就将这实例保存到这个字典中

```py
def singleton(cls):
    _instance = {}

    def _sigleton(*args, **kwargs):
        if cls not in _instance:
            _instance[cls] = cls(*args, **kwargs)
        return _instance[cls]
    return _sigleton

@singleton
class A:
    def __init__(self, x=0):
        self.x = x

# 这种装饰器可以传递参数
```

#### 3、使用类
调用类的instance方法，这样有个弊端，在使用类创建的时候，不是单例，必须在创建类的时候，使用类里面的规定的方法创建

```py
class SingletonThree:
    def __init__(self) -> None:
        pass

    @classmethod
    def get_instance(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = cls(*args, **kwargs)
        
        return cls._instance

threeone_ =  SingletonThree.get_instance()
threetwo_ =  SingletonThree.get_instance()

print(f"threeone_ id is {threeone_}")
print(f"threetwo_ id is {threetwo_}")
print(f"threeone_ is threetwo_ = {threeone_ is threetwo_}")

# threeone_ id is <utils.singleton.SingletonThree object at 0x0000028E16713D60>
# threetwo_ id is <utils.singleton.SingletonThree object at 0x0000028E16713D60>
# threeone_ is threetwo_ = True
```

#### 4、使用__new__方法实现(推荐)
使用3 类创建单例，必须使用cls._get_instance()方法，推荐使用__new__()方法来创建单例
```py
# 一个对象的实例化过程是先执行__new__方法，如果没写，默认调用object的__new__方法，返回一个
# 实例化对象,然后在调用__init__方法，对这个对象进行示例化。

# 在一个类的，__new__方法中 先判断是不是存在实例，如果存在实例就直接返回，不存在就创建。

def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super().__new__(cls)
        return cls._instance

```
### 工厂模式

工厂模式：简单工厂模式，允许接口创建对象，但是不会暴露对象创建逻辑复杂对象适用于工厂模式。
使用实例：  
1、日志记录器，记录到本地硬盘、系统事件、远程服务，可选记录日志到什么地方  
2、数据库访问，不知道最后采用哪一类数据库，以及数据库可能有变化时  
3、设计一个连接服务器的框架，将不同的协议共同实现到一个接口  

第一步：创建4个具体的类（没有或者不关心构造函数），用来创建具体的对象。

第二步：创建一个工厂类，根据名字来创建具体的对象

总结：简单工程模式，违背开放-封闭（对扩展开放，对修改关闭）原则，每次需要添加新对象的时候，都要修改工厂函数。

### 工厂方法模式

工厂方法模式解决了简单工厂模式违背的开放封闭原则
1、工厂方法模式定义一个用于创建对象的接口，但是工厂本身并不负责创建对象，而是让子类决定那一个类示例化，
工厂方法的创建是通过继承而不是通过实例化来完成的
2、每个工厂只负责生成产自己的产品，避免在新增产品时要修改工厂代码，新增产品，只需要增加相应的工厂

使用实例：  
1、系统中用于的子类很多，以后可能还需要不断拓展增加不同子类时  
2、当设计系统时，还不能明确具体有哪些类时
