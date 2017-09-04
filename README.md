# Study-notes
#闭包学习笔记
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()#count依次将函数f赋给f1,f2,f3
print(f1(),f2(),f3())#重新调用f1,f2,f3打印出值,调用f1,f2,f3时，此时i的值为3,故打印结果为9,9,9


def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
f1, f2, f3 = count()
print(f1(),f2(),f3())




import functools
def log():
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('begin call %s():' % ( func.__name__))
            return func(*args, **kw)         
        return wrapper
    return decorator
@log()
def now():
    print('2015-3-25')
print(now())

 
# 练习，编写一个decorator，能在函数调用的前后打印出'begin call'和'end call'的日志

import functools
def log():
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('begin call %s():' % ( func.__name__))
            func(*args, **kw)
            print('End call %s():' % ( func.__name__))
        return wrapper
    return decorator
@log()
def now():
    print('2015-3-25')
print(now())
#练习,能否写出一个@log的decorator，使它既支持
@log
def f():
    pass,
又 能够
@log('execute')
def f():
    pass
 
 import functools
from types import FunctionType
def log2(text):
    if isinstance(text, FunctionType):
        @functools.wraps(text)
        def wrapper(*args, **kw):
            print('call %s():' % text.__name__)
            return text(*args, **kw)

        return wrapper
    else:
        def decorator(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                print('%s %s():' % (text, func.__name__))
                return func(*args, **kw)

            return wrapper

        return decorator


@log2
def two():
    print("2017-08-31")


@log2("run")
def two2():
    print("2017-08-31")

two()
print('---------------------------------------------')
two2()


#偏函数
def int2(x, base=2):
    return int(x, base)
int2('1000000') #64
int2('1010101') #85

import functools
int2 = functools.partial(int, base=2)#functools.partial就是帮助我们创建一个偏函数的，不需要我们自己定义int2()，可以直接使用下面的代码创建一个新的函数
>>> int2('1000000')
64
>>> int2('1010101')
85

#模块

#!/usr/bin/env python3  #第1行注释可以让这个hello.py文件直接在Unix/Linux/Mac上运行
# -*- coding: utf-8 -*- #第2行注释表示.py文件本身使用标准UTF-8编码
 
' a test module '第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释

__author__ = 'Michael Liao'第6行使用__author__变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名

import sys  #导入模块
def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':#当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量__name__置为__main__，而如果在其他地方导入该hello模块时        test()                if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试


#面向对象
class Student(object):

    def __init__(self, name, score):#__init__方法的第一个参数永远是self，表示创建的实例本身，因此，在__init__方法内部，就可以把各种属性绑定到             self.name = name          self，因为self就指向创建的实例本身。__init__方法，在创建实例的时候，就不能传入空的参数了，必须传入与__init__           self.score = score         方法匹配的参数，但self不需要传，Python解释器自己会把实例变量传进去：
                              
       

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
bar=Student('Bart Simpson', 59)
print(bar.name)
print(bar.score)


    
