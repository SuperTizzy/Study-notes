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


#定制类
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__

 
#如果一个类想被用于for ... in循环，类似list或tuple那样，就必须实现一个__iter__()方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的__next__()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。

我们以斐波那契数列为例，写一个Fib类，可以作用于for循环

class Fib(object):
def __init__(self):
    self.a, self.b = 0, 1 # 初始化两个计数器a，b

def __iter__(self):
    return self # 实例本身就是迭代对象，故返回自己

def __next__(self):
    self.a, self.b = self.b, self.a + self.b # 计算下一个值
    if self.a > 100000: # 退出循环的条件
        raise StopIteration()
    return self.a # 返回下一个值

#要表现得像list那样按照下标取出元素，需要实现__getitem__()方法：
class Fib(object):
def __getitem__(self, n):
    a, b = 1, 1
    for x in range(n):
        a, b = b, a + b
    return a
    
    
    
    
    
    
#文件读写
# 由于文件读写时都有可能产生IOError，一旦出错，后面的f.close()就不会调用。所以，为了保证无论是否出错都能正确地关闭文件，我们可以使用try ... finally来实现：
try:
    f=open('D:/Downloads/hello.py','r')
    print(f.read())
finally:
    if f:
        f.close()
# 但是每次都这么写实在太繁琐，所以，Python引入了with语句来自动帮我们调用close()方法：
with open('D:/Downloads/hello.py','r') as f:
     print(f.read())
# 。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可
with open('D:/Pictures/IMG_20170829_083956.jpg''','rb') as f:
     print(f.read())
     
#数据读写不一定是文件，也可以在内存中读写 
  from io import StringIO
f=StringIO()
f.write('hello')
f.write('  ')
f.write('world')
print(f.getvalue())

f1=StringIO('hello!\nhi!\ngoodbye!')
while True:
    s=f1.readline()
    if s=='':
        break
    print(s.strip())

 
#通过os.path.split()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名：
 os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
# os.path.splitext()可以直接让你得到文件扩展名，很多时候非常方便：
os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')  #这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作。


#序列化
#我们把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，python通过pickle模块实现序列化
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)  #pickle.dumps()可以把任意对象序列化为bytes
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)   #pickle.dump()直接把对象序列化后写入一个file-like Object
>>> f.close()
#Pickle的问题和所有其他编程语言特有的序列化问题一样，就是它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据，不能成功地反序列化也没关系。如果我们要在不同的编程语言之间传递对象，就必须把对象序列化为标准格式，比如XML，但更好的方法是序列化为JSON，因为JSON表示出来就是一个字符串，可以被所有语言读取，也可以方便地存储到磁盘或者通过网络传输。JSON不仅是标准格式，并且比XML更快，而且可以直接在Web页面中读取，非常方便。
#将python对象变为json
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
#dumps()方法返回一个str，内容就是标准的JSON。类似的，dump()方法可以直接把JSON写入一个file-like Object。


#要把JSON反序列化为Python对象，用loads()或者对应的load()方法，前者把JSON的字符串反序列化，后者从file-like Object中读取字符串并反序列化：
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}

 
# 多线程
#当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量__name__置为__main__，而如果在其他地方导入该hello模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。
#multiprocessing适合跨平台编写多进程程序


from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))   #multiprocessing模块提供了一个Process类来代表一个进程对象
    print('Child process will start.')
    p.start()
    p.join()  #创建子进程时，只需要传入一个执行函数和函数的参数，创建一个Process实例，用start()方法启动，这样创建进程比fork()还要简单。
               join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步。
    print('Child process end.')
    
    
    
    
 #启动大量的子进程，可以用进程池的方式批量创建子进程
 from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()  #对Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了。
    p.join()
    print('All subprocesses done.')
    
    
 
# 进程间通信Process之间肯定是需要通信的，操作系统提供了很多机制来实现进程间的通信。Python的multiprocessing模块包装了底层的机制，提供了Queue、Pipes等多种方式来交换数据。我们以Queue为例，在父进程中创建两个子进程，一个往Queue里写数据，一个从Queue里读数据


from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()
    
# 多线程

import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)  #current_thread()函数永远返回当前线程实例
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')  
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name  #主线程实例的名字叫MainThread，子线程的名字在创建时指定，我们用LoopThread命名子线程
 
# 线程锁
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
 #当多个线程同时执行lock.acquire()时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止。
获得锁的线程用完后一定要释放锁，否则那些苦苦等待锁的线程将永远等待下去，成为死线程。所以我们用try...finally来确保锁一定会被释放。

#threadlocal 

import threading

# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()
# 全局变量local_school就是一个ThreadLocal对象，每个Thread对它都可以读写student属性，但互不影响。你可以把local_school看成全局变量，但每个属性如local_school.student都是线程的局部变量，可以任意读写而互不干扰，也不用管理锁的问题，ThreadLocal内部会处理。
可以理解为全局变量local_school是一个dict，不但可以用local_school.student，还可以绑定其他变量，如local_school.teacher等等。
ThreadLocal最常用的地方就是为每个线程绑定一个数据库连接，HTTP请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。







 #正则表达式
# 验证下面Email的正则表达式
# someone@gmail.com
#bill.gates@microsoft.com
import re
re_email=re.compile(r'([\w.]+)@(\w+)\.com')
print(re_email.match('someone@gmail.com').groups())
print(re_email.match('bill.gates@microsoft.com').groups())
print(re_email.match('819447987@qq.com').groups())
 
#版本二可以验证并提取出带名字的Email地址：
import re
re_email=re.compile(r'([<\w\s>]+)\s(\w+)@(\w+)\.[com,cn,org]')
print(re_email.match('<Tom Paris> tom@voyager.org').groups())



#python 模块
#datetime

#获取当前日期和时间
>>> from datetime import datetime
>>> now = datetime.now() # 获取当前datetime
>>> print(now)
2015-05-18 16:28:07.198690
>>> print(type(now))
<class 'datetime.datetime'>
 
#获取指定日期和时间
>>> from datetime import datetime
>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
>>> print(dt)
2015-04-19 12:20:00
 
#datetime转换为timestamp
>>> from datetime import datetime
>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
>>> dt.timestamp() # 把datetime转换为timestamp
1429417200.0
 
# timestamp转换为datetime
>>> from datetime import datetime
>>> t = 1429417200.0
>>> print(datetime.fromtimestamp(t))
2015-04-19 12:20:00

>>> from datetime import datetime
>>> t = 1429417200.0
>>> print(datetime.fromtimestamp(t)) # 本地时间
2015-04-19 12:20:00
>>> print(datetime.utcfromtimestamp(t)) # UTC时间
2015-04-19 04:20:00
 
# str转换为datetime
>>> from datetime import datetime
>>> cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
>>> print(cday)
2015-06-01 18:19:59
 
#datetime转换为str
>> from datetime import datetime
>>> now = datetime.now()
>>> print(now.strftime('%a, %b %d %H:%M'))
Mon, May 05 16:28

 
# datetime加减
>>> from datetime import datetime, timedelta
>>> now = datetime.now()
>>> now
datetime.datetime(2015, 5, 18, 16, 57, 3, 540997)
>>> now + timedelta(hours=10)
datetime.datetime(2015, 5, 19, 2, 57, 3, 540997)
>>> now - timedelta(days=1)
datetime.datetime(2015, 5, 17, 16, 57, 3, 540997)
>>> now + timedelta(days=2, hours=12)
datetime.datetime(2015, 5, 21, 4, 57, 3, 540997)

 
#本地时间转换为UTC时间
>>> from datetime import datetime, timedelta, timezone
>>> tz_utc_8 = timezone(timedelta(hours=8)) # 创建时区UTC+8:00
>>> now = datetime.now()
>>> now
datetime.datetime(2015, 5, 18, 17, 2, 10, 871012)
>>> dt = now.replace(tzinfo=tz_utc_8) # 强制设置为UTC+8:00
>>> dt
datetime.datetime(2015, 5, 18, 17, 2, 10, 871012, tzinfo=datetime.timezone(datetime.timedelta(0, 28800)))
 
#时区转换
# 拿到UTC时间，并强制设置时区为UTC+0:00:
>>> utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
>>> print(utc_dt)
2015-05-18 09:05:12.377316+00:00
# astimezone()将转换时区为北京时间:
>>> bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
>>> print(bj_dt)
2015-05-18 17:05:12.377316+08:00
# astimezone()将转换时区为东京时间:
>>> tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt)
2015-05-18 18:05:12.377316+09:00
# astimezone()将bj_dt转换时区为东京时间:
>>> tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt2)
2015-05-18 18:05:12.377316+09:00
 
 #collections模块
 (1) cnamedtuple
namedtuple是一个函数，它用来创建一个自定义的tuple对象，并且规定了tuple元素的个数，并可以用属性而不是索引来引用tuple的某个元素。这样一来，我们用namedtuple可以很方便地定义一种数据类型，它具备tuple的不变性，又可以根据属性来引用，使用十分方便。
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2

 (2) deque
 使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为list是线性存储，数据量大的时候，插入和删除效率很低。deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈，deque除了实现list的append()和pop()外，还支持appendleft()和popleft()，这样就可以非常高效地往头部添加或删除元素。
 >>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])

 
(3)defaultdict
使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict：
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'
注意默认值是调用函数返回的，而函数在创建defaultdict对象时传入。

除了在Key不存在时返回默认值，defaultdict的其他行为跟dict是完全一样的。

(4)OrderedDict
使用dict时，Key是无序的。在对dict做迭代时，我们无法确定Key的顺序,如果要保持Key的顺序，可以用OrderedDict
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
注意，OrderedDict的Key会按照插入的顺序排列，不是Key本身排序：
OrderedDict可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key

from collections import OrderedDict
class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity
    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
 

#TCP编程
# 创建一个基于TCP连接的socket
# 导入socket库:
import socket

# 创建一个socket:
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  #创建Socket时，AF_INET指定使用IPv4协议,SOCK_STREAM指定使用面向流的TCP协议
# 建立连接:
s.connect(('www.sina.com.cn', 80))  #新浪网站的IP地址可以用域名www.sina.com.cn自动转换到IP地址,80端口是Web服务的标准端口
# 发送数据:
s.send(b'GET / HTTP/1.1\r\nHost: www.sina.com.cn\r\nConnection: close\r\n\r\n')
# 接收数据:
buffer = []
while True:
    # 每次最多接收1k字节:
    d = s.recv(1024)  #调用recv(max)方法，一次最多接收指定的字节数，因此，在一个while循环中反复接收，直到recv()返回空数据，表示接收完毕，退出循环
    if d:
        buffer.append(d)
    else:
        break
data = b''.join(buffer)
# 关闭连接:
s.close()


header, html = data.split(b'\r\n\r\n', 1)
print(header.decode('utf-8'))
# 把接收的数据写入文件:
with open('sina.html', 'wb') as f:
    f.write(html)
 
 #一个简单的服务器程序，它接收客户端连接，把客户端发过来的字符串加上Hello再发回去
import socket
import threading
import time
# 每个连接都必须创建新线程（或进程）来处理，否则，单线程在处理连接的过程中，无法接受其他客户端的连接
def tcplink(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Welcome!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        sock.send(('Hello, %s!' % data.decode('utf-8')).encode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)
    
 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 创建一个基于IPv4和TCP协议的Socket
s.bind(('127.0.0.1', 9999))  # 监听端口,127.0.0.1是一个特殊的IP地址，表示本机地址，如果绑定到这个地址，客户端必须同时在本机运行才能连接，也就是                                 说，外部的计算机无法连接进来,因为我们写的这个服务不是标准服务，所以用9999这个端口号。请注意，小于1024的端口号必须要                               有管理员权限才能绑定

s.listen(5)# 紧接着，调用listen()方法开始监听端口，传入的参数指定等待连接的最大数量
print('Waiting for connection...')
while True:
    # 接受一个新连接:
    sock, addr = s.accept()
    # 创建新线程来处理TCP连接:
    t = threading.Thread(target=tcplink, args=(sock, addr))
    t.start()

#要测试这个服务器程序，我们还需要编写一个客户端程序
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 建立连接:
s.connect(('127.0.0.1', 9999))   
# 接收欢迎消息:
print(s.recv(1024).decode('utf-8'))
for data in [b'Michael', b'Tracy', b'Sarah']:
    # 发送数据:
    s.send(data)
    print(s.recv(1024).decode('utf-8'))
s.send(b'exit')
s.close()
 
 




