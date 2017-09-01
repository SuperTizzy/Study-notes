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
