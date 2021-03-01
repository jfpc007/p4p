# P4P
程序员学Python笔记

## chap1 
#### 1 算数运算
 
> 取余：整点数和浮点数都能取余（p2）
 
> 规则：余数(r) = 被除数(a) - n*(a // n)
 
```
-7 = 123 % （-10）
   = 123 - (-10)(123 // -10) 
   = 123 - (-10)(-13) # // 向下取整
 
 7 = -123 % 10
   = -123 - 10（-123 // (-10)）
   = -123 - 10*12
  
 -3 = -123 % (-10)
```
> 负号与乘幂
```
-5 ** -3 # -0.008
```

> Python取整——向上取整、向下取整、四舍五入取整、向0取整 <br>
> refer to: https://blog.csdn.net/weixin_41712499/article/details/85208928 <br>
向上取整: math.ceil()
```
from math import ceil
ceil(-0.5) # 0
ceil(-0.9) # 0
ceil(0.3) # 1
```
向下取整: floor()
```
from math import floor
floor(-0.3) # -1
floor(0.9) # 0
```
银行家舍入取整: round(number)
```
# 四舍五入考虑：
# .5后非空进1
# .5后为空：5前为偶舍去，为奇进1
round(-2.5) # -2 
round(-1.5) # -2
```
向0取整: int()
```
# 丢掉小数，取整数部分
int(-0.9) # 0
int(0.5) # 0
```
除法取整: //(向下取整)
```
(-1) // 2  # -0.5 to -1
(-3) // 2  # -1.5 to -2
1 // 2    # 0.5 to 0
3 // 2    # 1.5 to 1
```

Python 浮点数 <br>
Python能够处理任意大的整数，精度有限的双精度浮点数（16~17位十进制精度）
> 小数通常以浮点数的形式存储 <br>
> 十进制形式 34.6 <br>
> 指数形式 2.1E5 = 2.1×105 <br>

#### 2 字符串操作
切片
- s[m:n]  # m >= n None
- s[m:n:d] # d是正数但 m >= n, d是负数但 n>=m 时 None

运算符优先级 <br>
(or < and < not)逻辑 < 比较运算符 < 算数

条件表达式 <br>
把 x 的绝对值赋值给 y ： `y = x if x >= 0 else -x`

程序控制和短路求值 <br>
a, b 是任意表达式，逻辑运算求值如下：
- a and b：先求a，如果a值为假，就以这个值作为整个 and 表达式的值；a不假再求b，以b值作为 and 表达式的值。
- a or b：先求a， 如果a值为真，就以这个值作为 or 表达式的值；否则求b，并以其值作为表达式的值。
- not a
```
3 and 0 # 0
0 or 'abc' # 'abc'
if x > 0 and y/x > 1: ...x...y
```
循环描述只在 for 语句开始时求值一次：
```
# i = 0, 1, 8
# n = 12
n = 3
for i in range(n):
    print(i**3)
    n = n + 3
```

#### 3 递归函数
递归函数：在函数体里调用自身，也是一种循环 <br>
> 思路： 基本情况直接给出结果，一般情况通过递归处理 <br>
> 例子 自然数指数乘幂: x**n
```
# 方法1 基本定义
def power(x, n):
    if n == 0:
        return 1
    else:
        return x * power(x, n-1)

# 方法2 条件表达式
def power(x, n):
    return 1 if n == 0 else x * power(x, n-1)

# 方法3 指数为奇转化为偶数，减少递归次数
def power(x, n):
    if n == 0:
        return 1
    elif n % 2 != 0: # n > 0 且为奇数
        return x * power(x, n-1)
    else:
        return power(x*x, n//2) # n > 0 且为偶数

# 循环实现（Python 既有循环也有递归结构）
def power(x, n):
    p = 1
    while n > 0:
        if n % 2 != 0:
            p *= x
            n -= 1
        else:
            x *= x
            n //= 2 # n 最终归结为奇数 1，转到 if 分支
    return p
```

递归、循环和执行时间 p27
> Fibonacci二路递归 循环不变式 平行赋值 <br>
> 递归定义，运算速度极慢
```

def fib(n):
    if n < 1: # F0 = 0
        return 0
    if n == 1: # F1 = 1
        return 1
    return fib(n - 1) + fib(n - 2) # Fn = F(n-1) + F(n-2) 
```
> improved: Loop + 平行赋值 <br>
> **循环不变式**证明 while LOOP p29 <br>
> 0 如果 每次判断循环条件时 f1 的值总是第 k 个斐波那契数，而 f2 的值总是第 k+1 个斐波那契数， 当 k=n 时循环结束，f1 就一定是第 n 个斐波那契数了。<br>
> 1 初始化中，f1==0，也即 F0，且 f2 的值是 F1，是下一个斐波那契数，所需关系成立。<br>
> 2 假设某次判断循环条件时，f1 的值是 Fk 且 f2 的值是Fk+1。循环体里的赋值使 f1 f2 分别变成 Fk+1 Fk+2，随后变量 k 的值加1，f1 f2 重新变成 Fk Fk+1。因此在下一次判断循条件时，所需关系仍然成立。<br>
```
# while LOOP
 def fib(n):
     if n < 0:
         return 0
     f1, f2 = 0, 1
     k = 0
     while k < n:
         f1, f2 = f2, f2 + f1
         k += 1
     return f1

# for LOOP
def fib(n):
    f1, f2 = 0, 1
    for k in range(n):
        f1, f2 = f2, f2 + f1
    return f1
    
# 递归
def fib0(f1, f2, k, n):
    if k > n:
        return f1
    else:
        return fib0(f2, f2+f1, k+1, n)
def fib(n):
    return fib0(0, 1, 1, n)
    
# list F0~Fn
def gen_fibs(n):
    fibs = [0] * (n + 1)
    fibs[1] = 1
    for i in range(2, n+1):
        fibs[i] = fibs[i-1] + fibs[i-2]
    return fibs

def gen_fibs1(n):
    fibs = [0, 1]
    for i in range(2, n+1):
        fibs.append(fibs[-2] + fibs[-1])
    return fibs
 
fs = gen_fibs(20)
```

#### 4 程序框架（函数嵌套p38 高阶函数p41、p69）
> 通用求根函数（立方根、平方根）
```
def appr_method(x, not_enough, improve):
    # 逼近计算框架函数，高阶函数
    if x == 0.0:
        return 0.0
    
    guess = x
    while not_enough(x, guess):
        guess = improve(x, guess)
    return guess
    
def not_enough(x, guess): # 通用判断逼近程度
    return abs((guess**3 - x) / x) > 1e-6
    
def cbrt(x):
    # 求立方根
    def improve(x, guess): # 函数嵌套
        retturn (2.0 * guess + x / guess / guess) / 3
        
    return appr_method(x, not_enough, improve)

def sqrt(x):
    # 求平方根
    def improve(x, guess):
        return (guess + x / guess) / 2
        
     return appr_method(x, not_enough, improve)
 ```
 
 > 匿名函数p45 lambda <br>
 > lambda 表达式不能出现赋值和函数定义 <br>
 > 可以有参数，参数即局部变量 <br>
 > 可以使用外围作用于中的变量 <br>
 ```
 def cbrt(x):
     return appr_method(x, not_enough, 
                        lambda x, y: (2.0 * y + x / y / y) / 3)
 ```
 
> 全局声明 <br>
> 希望在一个函数里修改全局定义的变量(global) 或 外围函数里的定义变量(nonlocal) <br>
> 函数内把 x 声明为全局变量， 如果执行到给 x 赋值时全局名字空间里没有 x ，解释器会把 x 加进全局名字空间并赋值 <br>
> e.g.cbrt: 统计在一次立方根计算中所有函数的调用次数 p52
```
count = 0
def cbrt(x):
    global count

    def not_enough(x, guess):
        global count
        count += 1
        return abs((guess**3 - x) / x) > 1e-6

    improve(x, guess):
        global count
        count += 1
        return (2.0 * guess + x / guess / guess) / 3

    count = 1

    if x == 0.0:
        return 0.0
    guess = x
    while not_enough(x, guess):
        guess = improve(x, guess)
        return guess
```

## chp2 数据的构造和组织
#### 1 list
#### 2 tuple
tuple: `t = 1, # t = (1, )` <br>
packing:`tp  = 123, 345, 'but', 567`, unpacking:`a, b, c, d = tp` p73 <br>
如果函数 fun(...) 返回一个二元组，`x, y = fun(...)`,则 x y 分别得到 fun 返回值里的两个成分。<br>
平行赋值 `x, y = 0, 1` 是左边拆分右边打包 <br>
> 有理数类p218 p78
```
class Rational:
    @staticmethod
    def _gcd(m, n):
        # 求最大公约数
        if n == 0:
            m, n = n, m
        while m != 0:
            m, n = n % m, m
        return n
    def __init__(self, num, den=1):
        if not (isinstance(num, int) and isinstance(den, int)):
            raise TypeError
        if den == 0:
            raise ZeroDivisionError
        sign = 1
        if num < 0:
            num, sign = -num, -sign
        if den < 0:
            den, sign = -den, -sign
        g = Rational._gcd(num, den) # 化简，无公约数
        self._num = sign * (num // g) # 规范 负号显示在分子上
        self._den = den // g
    def num(self):
        return self._num
    def den(self):
        return self._den
        
    # 特殊方法名 + * //
    def __add__(self, another):
        num = (self._num * another.den() +
               self._den * another.num())
        den = self._den * another.den()
        return Rational(num, den)
        
    def __mul__(self, another):
        return Rational(self._num * another.num(),
                        self._den * another.den())
                        
    def __floordiv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        return Rational(self._num * another.den(),
                        self._den * another.num())
     
    # other: -:__sub__, %:__mod__ ...
    
    # /:__truediv__ 有理数转换浮点数
    def __turediv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        rel = （(self._num * another.den())/
              (self._den * another.sun())）
        return rel
        
    # logical operation
    def __eq__(self, another):
        # num den 均通过 Rational 化简得到，不须通分比较
        return (self._num == another.num() and
                self._den == another.den())
        
    def __lt__(self, another):
        return (self._num * another.den() <
                self._den * another.num())
    # other: !=:__ne__, <=:__le__, >:__gt__, >=:__ge__
    
    def __str__(self):
        # print() 输出
        return str(self._num) + '/' + str(self._den)

if __name__ == "__main__":
    five = Rational(5)
    x = Rational(3, 5)
    print('Two third are', Rational(2, 3)) # Two third are 2/3
    y = five + x * Rational(5, 17) 
    if y > Rational(123, 11): print('It is large.')
    t = type(five) # <class '__main__.Rational'>
    if isinstance(five, Rational): print('It is OK.')
```
#### 3 序列操作
可变序列操作：
`s[0:0] = t` 在 s 头添加一段元素， t 是任意可迭代对象。
`s[len(s):len(s)] = t` 和 `s.extent(t)` 在 s 尾添加一段元素。
```
s = [1, 2, 3]
s[0:0] = '01'
print(s) # ['0', '1', 1, 2, 3]
```

    
