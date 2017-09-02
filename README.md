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
