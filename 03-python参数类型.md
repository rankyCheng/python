# 函数的参数类型
## 可分为：必须参数、默认参数、可变参数（不定长参数）、关键字参数.
### 必须参数
>   def hello(str):                                                                    
>	     print('必须参数是：',str)                                                             
>   hello('hello,world')                                                           

在函数hello中，str作为一个参数，是形参，这形参个类型取决于你调用时输入的实参类型                                                           
调用函数hello时，传入了一个字符串'hello,world'，那么这个实参的类型就是字符串，所以形参str的类型也是字符串                                                  
当我们调用函数hello时传入‘hello,world’，就会执行函数里面的print语句，返回: 必须参数是： hello,world                                              

### 默认参数:定义函数时，形参给定一个值。
-   调用的时候注意顺序要正确。
>    def hs(name,age=23):                                                           
>	      print('我叫：',name)                                                     
>	      print('我今年',age)                                                  
>    hs('王二',23)                                                  


-   有多个默认参数时，调用时既可以按顺序提供默认参数，也可以不按顺序提供部分默认参数。但当不按顺序提供部分默认参数时，参数名要写上
>   def enroll(name, gender, age=6, city='Beijing'):                                          
>      print('name:', name)                                                     
>      print('gender:', gender)                                                                 
>      print('age:', age)                                                                 
>      print('city:', city)                                                   
>   enroll('Bob', 'M', 7)              
>   enroll('Adam', 'M', city='Tianjin')

###  可变参数（不定长参数*args）
-   实参name传给了形参age，而其余三个传给了*som，可以说som相当于一个无限大的容器，可以容纳很多个参数。
>    def change(age,*som):                                                                           
>      print(age)                                                                                  
>      for i in som:                                                                              
>        print(i)                                                                                      
>    return                                                                                    
-    返回--change('name','year','mon','address')                                 

-   当形参中有不定长参数 *other，调用函数时用 *dir会发现结果是只有字典中键名，没有值
>    dir={'name': 'miss', 'age': '18'}                                                                    
>       def Deaf(school, banji, *other):                                                                        
>          print('Xuexiao:', school, 'Banji:', banji, 'Student_info:', other)                                                         
>    Deaf('Tsinghua', 'Class 2', *dir)                                                                                    
-    返回--Xuexiao: Tsinghua Banji: Class 2 Student_info: ('name', 'age')                                                            

-   当形参有不定长参数 *other ，调用函数时用 dir 但其结果是 将字典 以元组的形式输出，既在字典外面加括号。
>   dir={'name': 'miss', 'age': '18'}                                                         
>   def Deaf(school, banji, *other):                                                                      
>      print('Xuexiao:', school, 'Banji:', banji, 'Student_info:', other)                                                               
>   Deaf('Tsinghua', 'Class 2', dir)                                                                                  
-   返回--Xuexiao: Tsinghua Banji: Class 2 Student_info: ({'name': 'miss', 'age': '18'},)                                                                     

### 关键字参数(**kw):在调用函数时，传入实参时带参数名，用这样的方式传入的实参叫做关键字参数。
-   和可变参数类似，也可以先组装出一个dict，然后，把该dict转换为关键字参数传进去
>    dir={'name': 'miss', 'age': '18'}                       
>    def Deaf(school, banji, **other):                       
>        print('学校 :', school, '班级:', banji, '学生信息:', other) 


###  总结
-   参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
>    def f1(a, b, c=0, *args, **kw):
>        print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

调用方式及结果分别:
>    f1(1, 2)                                                       
>    a = 1 b = 2 c = 0 args = () kw = {}                                                        

>    f1(1, 2, c=3)                                                  
>    a = 1 b = 2 c = 3 args = () kw = {}                                                                        

>    f1(1, 2, 3, 'a', 'b')                                                
>    a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}                                                          

>    f1(1, 2, 3, 'a', 'b', x=666)                                                     
>    a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 666}                                                                                                 

-   如何定义可变参数和关键字参数                                                                                    
*args是可变参数，args接收的是一个tuple；**kw是关键字参数，kw接收的是一个dict。                                                 
使用*args和**kw是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。(kw=key word)                                                  
通过一个tuple和dict，上述函数也可以这么调用：                                                           
>    args = (1, 2, 3, 4)
>    kw = {'d': 99, 'x': '#'}
>    f1(*args, **kw)
>    a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}

-   调用函数时如何传入可变参数和关键字参数:
-   可变参数既可以直接传入：func(1, 2, 3)，又可以先组装tuple，再通过*args传入：func(*(1, 2, 3))；
-   关键字参数既可以直接传入：func(a=1, b=2)，又可以先组装dict，再通过**kw传入：func(**{'a': 1, 'b': 2})。





