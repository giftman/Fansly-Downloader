##《Python核心编程2》

###第14章 执行环境

- 可调用对象 --> 加()可执行
 - 函数（内建BIF build_in_function,用户定义udf,lambda)
 - 方法 (BIM,UDM)
 - 类 (调用创建了实例)
 - 类的实例   实现了__call__的类的实例

- 代码对象 代码块/udf.func_code/方法(函数对象(代码对象))

- callable,compile(eval求值,single单一,exec语句组)

- eval/exec (执行代码对象)

```bash
>>> exec """
... x = 0
... print 'x is currently:',x
... """
x is currently: 0
>>> f = open('xcount.py')
```

- input 等同于 eval(raw_input(prompt=''))  返回python对象,raw_input返回string

```bash
>>> aString = raw_input('Enter a list')
Enter a list[123,'xyz',46.7]
>>> aString
"[123,'xyz',46.7]"
>>> aList = input('Enter a list:')
Enter a list:[123,'xyz',46.7]
>>> aList
[123, 'xyz', 46.7]
```
- 函数属性

```python
#!/usr/bin/env python

def foo():
    return True

def bar():
    'bar() dose not do much'
    return True

foo.__doc__ = 'foo() does not do much'
foo.tester='''
if foo():
    print 'PASSED'
else:
    print 'FAILED'
'''

for eachAttr  in dir():
    obj = eval(eachAttr)
    if isinstance(obj,type(foo)):
        if hasattr(obj,'__doc__'):
            print '\nFunction "%s" has a doc string:\n\t%s' % (eachAttr,obj.__doc__)
        if hasattr(obj,'tester'):
            print 'Function "%s" has a tester... executing' % eachAttr
            exec obj.tester
        else:
            print 'Function "%s" has no tester... skiping' % eachAttr
    else:
        print '"%s" is not a function ' % eachAttr
```

- 执行其它Python程序
 - import /execfile()
 
    ```bash
    f = open(filename,'r')
    exec f
    f.close()
    ```
- 执行其它非Python程序--->subprogress
 - subprogress.call(['ls','-al'])
 - Popen 
 
 ```bash
 >>> from subprocess import PIPE,Popen
 >>> f = Popen(['ls','-al'],stdout=PIPE).stdout
 >>> for each in f:
 ...     print each
 ... 
 total 96

 drwxr-xr-x@ 19 winfan  staff   646 Oct 12 11:15 .

 drwxr-xr-x   7 winfan  staff   238 Aug 28 10:28 ..

 -rw-r--r--   1 winfan  staff   380 Aug 19 10:06 .project
 ```

 ```python
#stdin stdout have PIPE will return an object,else None
# p= Popen(['python','test1.py'],stdin=PIPE,stdout=PIPE)
#p.write('test')
#p.communicate()[0] cause in&out/it's PIPE so only communicate can flush to stout
#如果是ls -al 这种命令，则直接运行完毕并关闭pip 已无法再操作in,out
#Both subprocess and os.popen* only allow input and output one time, and the output to be read only when the process terminates.
 ```
 
  - 根据unix 规范 要编写从stdin 获取输入再输出到stdout 直接借助sys.stdin就可以 
  
  ```python
#test1.py
#!/usr/bin/env python
import sys
line = sys.stdin.readlines()
print 'test',line
  ```
  
  ```bash
    practise git:(master) ✗ ls | python test1.py
test ['11_11.py\n', '11_12.py\n'] 
  ```
  
- 结束执行 主动 sys.exit(status=0) 需要退出前清理设置 sys.exitfunc()



###第15章 正则表达式
- compile 多则预编译
- group 匹配到的  groups 子组 group(1)也是子组

```bash
>>> f = re.match('(a)(b)','ab')
>>> f.group()
'ab'
>>> f.group(1)
'a'
>>> f.group(2)
'b'
>>> f.groups()
('a', 'b')ab'

```

- match,search,findall 
match & search 返回找到的第一个
match 要从头开始匹配 中间的还要捕获
search 可中间含有
两者都返回一个匹配对象

- findall 和search功能一样，返回列表
一般应该是想用search ,需要全部就findall
- split 正则分割
- raw strings 原始字符串 r''



###第18章 多线程编程

- 进程和线程：一个进程可以有多个线程

- thread + lock

- threading join 自处理锁（守护进程,setDaemon为false）

- threading 创建一个Thread的实例，可以传给它一个函数，可调用的类对象，或者派生出一个子类，实现它的run方法。书中建议第三种。
- Queue 共享线程间数据 put get 可以block


```python
#!/usr/bin/env python
#myThread.py

import threading
from time import ctime

class MyThread(threading.Thread):
    def __init__(self,func,args,name=''):
        threading.Thread.__init__(self)
        self.name = name
        self.func = func
        self.args = args
        self.res = ''

    def getResult(self):
        return self.res


    def run(self):
        print 'starting ',self.name,'at:',ctime()
        self.res = self.func(*self.args)
        print 'finished ',self.name,'at:',ctime()
```

```python
#!/usr/bin/env python
#mtsleep.py

import threading
from time import sleep,ctime
from myThread import MyThread
loops=[4,2]

class ThreadFunc(object):
    def __init__(self,func,args,name=''):
        self.name = name
        self.func = func
        self.args = args

    def __call__(self):
        self.func(*self.args)

def loop(nloop,nsec):
    print 'start loop',nloop,'at:',ctime()
    sleep(nsec)
    print 'loop',nloop,'done at:',ctime()

def fib(x):
    sleep(0.005)
    if x < 2: return 1
    return (fib(x-2) + fib(x-1))

def fac(x):
    sleep(0.1)
    if x<2:return 1
    return (x* fac(x-1))

def sum(x):
    sleep(0.1)
    if x<2:return 1
    return (x+sum(x-1))

def main():
    funcs = [fib,fac,sum]
    nfuncs = range(len(funcs))
    n=12
    print '*** SINGLE THREAD',ctime()
    for i in nfuncs:
        print 'starting',funcs[i].__name__, 'at:',ctime()
        print funcs[i](n)
        print funcs[i].__name__,'finished at:',ctime()

    print '\n*** MULTIPLE THREADS'
    threads = []

    for i in nfuncs:
        t = MyThread(funcs[i],(n,),funcs[i].__name__)
        threads.append(t)

    for i in nfuncs:
        threads[i].start()

    for i in nfuncs:
        threads[i].join()
        #print threads[i].getResult()
    for i in nfuncs:
        print threads[i].getResult()


    print 'all Done!'

if __name__ == '__main__':
    main()
```


