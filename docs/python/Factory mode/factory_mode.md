---
tags:
  - 工厂模式
  - 工厂方法模式
---


## 简单工厂模式
```py
# 实例化的类
class Cat:
    def say(self):
        return "cat: Hell,World!"

class Dog:
    def say(self):
        return "dog: Hell,World!"

class Pig:
    def say(self):
        return "pig: Hell,World!"

# 工厂类，用来创建具体的对象
class AnimFactory:
    def create_anim(self, name):
        if name == "Cat":
            return Cat()
        elif name == "Dog":
            return Dog()
        elif name == "Pig":
            return Pig()
        else:
            return None
if __name__ == '__main__':
    animfactory = AnimFactory()
    cat = animfactory.create_anim("Cat")
    dog = animfactory.create_anim("Dog")
    pig = animfactory.create_anim("Pig")

    print(f"cat->: {cat.say()}")
    print(f"dog->: {dog.say()}")
    print(f"pig->: {pig.say()}")

```

## 工厂方法模式
```py
from abc import ABC, abstractclassmethod
# 工厂方法模式
# 1、使用上面具体进行实例化的类Cat\Dog\Pig
# 2、抽象类，其子类就是工厂
class AbsFactory(ABC):
    # 抽象方法
    @abstractclassmethod
    def create_anim(self):
        pass

class CatFactory(AbsFactory):
    """ Cat 工厂"""
    def create_anim(self):
        return Cat()

class DogFactory(AbsFactory):
    """ Dog 工厂"""
    def  create_anim(self):
        return Dog()

class PigFactory(AbsFactory):
    """ Pig 工厂"""
    def create_anim(self):
        return Pig()


if __name__ == "__main__":

    print(f"#"*20)
    print(f"#")
    print(f"#   工厂方法模式")
    print(f"#")
    print(f"#"*20)

    cat = CatFactory().create_anim()
    dog = DogFactory().create_anim()
    pig = PigFactory().create_anim()
    print(f"cat {cat.say()}")
    print(f"dog {dog.say()}")
    print(f"pig {pig.say()}")
	

```