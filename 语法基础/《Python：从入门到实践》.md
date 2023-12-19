# 第二章   变量和简单数据类型

## 2.2 变量

```python
message = "Hello Python world!"
print(message)
```



**变量是标签**

变量是可以赋给值的标签，也可以说变量指向特定的值



## 2.3 字符串

字符串就是一系列字符

在Python中，用引号括起的都是字符串，其中的引号可以是单引号，也可以是双引号

```python
"This is a string."
'This is also a string'
```



**1. 使用方法修改字符串的大小写**

```python
name = "ada lovelace"
print(name.title())
```

输出：Ada  Lovelace



还有upper和lower方法



**2. 在字符串中使用变量**

在字符串中使用变量的值，可在前引号前加上字母f，再将要插入的变量放在花括号内

这种字符串名为f字符串，f是format（设置格式）的简写

```python
first_name = "ada"
last_name = "love"
full_name = f"{first_name} {last_name}"
```



**3. 使用制表符或换行符来添加空白**

在编程中，空白泛指任何非打印字符，如空格、制表符和换行符



要在字符串中添加制表符，可使用字符组合\t

要在字符串中添加换行符，可使用字符组合\n

```python
print(\tPython)
print(Python\n)
```



**4. 删除空白**

删除空白可用lstrip和strip



**5. 使用字符串时避免语法错误**

例如，在用单引号括起的字符串中，如果包含撇号，就将导致错误



## 2.4 数

Python能根据数的用法以不同的方式处理它们



1. **整数**

Python使用两个乘号表示乘方运算

```python
#输出为9
3**2
```



2. **浮点数**

Python将所有带小数点的数称为浮点数

但需要注意的是，结果包含的小数位数可能是不确定的

```python
0.2+0.1  //结果0.3000000000004
3*0.1    //结果0.3000000000004
```



3. **整数和浮点数**

将任意两个数相除时，结果总是浮点数，即便这两个数都是整数且能整除

只要有操作数是浮点数，Python默认得到的总是浮点数



4. **数中的下划线**

书写很大的数时，可使用下划线将其中的数字分组



5. **同时给多个变量赋值**

```python
x,y,z = 0, 0, 0
```



6. **常量**

常量类似于变量，但其值在程序的整个生命周期内保持不变

但Python程序员会使用全大写来指出应将某个变量视为常量，其值应始终不变

```python
MAX_CONNECTIONS = 5000
```



## 2.5 注释

1. **如何编写注释**

```python
# 向大家问好
print("Hello Python people!")
```



2. **该编写什么样的注释**

编写注释的主要目的是阐述代码要做什么，以及是如何做的



## 2.6 Python之禅

Python社区的理念都包含在Tim Peters撰写的“Python之禅”中

要获悉这些有关编写优秀Python代码的指导原则，只需在解释器中执行命令import this



* Beautiful is better than ugly
* Simple is better than complex
* Complex is better than complicated
* Readability counts
* There should be one-- and preferably only one --obvious way to do it
* Now is better than never



# 第三章 列表简介

## 3.1 列表是什么

列表由一系列按特定顺序排列的元素组成

在Python中，用方括号（[]）表示列表，并用逗号分隔其中的元素

```python
bicycles = ['trek', 'cannondale', 'specialized']
```

如果让Python将列表打印出来，Python将打印列表的内部表示，包括方括号



1. **访问列表元素**

列表是有序集合，因此要访问列表的任意元素，只需将该元素的位置（索引）告诉Python即可

```python
bicycles = ['trek', 'cannondale', 'specialized']
print(bicycles[0])
```



2. **索引从0而不是1开始**

在Python中，第一个列表元素的索引为0，而不是1

通过将索引指定为-1，可让Python返回最后一个列表元素

索引-2返回倒数第二个列表元素，索引-3返回倒数第三个列表元素，依此类推



3. **使用列表中的各个值**

你可以像使用其他变量一样使用列表中的各个值

```python
bicycles = ['trek', 'cannondale', 'specialized']
message = f"My first bicycle was a {bicycles[0].title()}."
print(message)
```



## 3.2 修改、添加和删除元素

1. **修改列表元素**

要修改列表元素，可指定列表名和要修改的元素的索引，再指定该元素的新值



2. **在列表中添加元素**

在列表中添加新元素时，最简单的方式是将元素附加（append）到列表

给列表附加元素时，它将添加到列表末尾

```python
bicycles.append("merida")
```



使用方法insert()可在列表的任何位置添加新元素

为此，你需要指定新元素的索引和值

```python
bicycles.insert(0,"merida")
```

merida被插入到了列表开头

方法insert()在索引0处添加空间，并将值'merida'存储到这个地方

这种操作将列表中既有的每个元素都右移一个位置



3. **从列表中删除元素**

* 如果知道要删除的元素在列表中的位置，可使用del语句

```python
del bicycles[0]
```



* 方法pop()删除列表末尾的元素，并让你能够接着使用它

```python
pop_value = bicycles.pop()
```



* 如果只知道要删除的元素的值，可使用方法remove()

使用remove()从列表中删除元素时，也可接着使用它的值

方法remove()只删除第一个指定的值

如果要删除的值可能在列表中出现多次，就需要使用循环来确保将每个值都删除



## 3.3 组织列表

Python提供了很多组织列表的方式，可根据具体情况选用



* 方法sort()永久性地修改列表元素的排列顺序

还可以按与字母顺序相反的顺序排列列表元素，只需向sort()方法传递参数reverse=True即可

```python
bicycles.sort(reverse = True)
```



* 使用函数sorted()对列表临时排序

要保留列表元素原来的排列顺序，同时以特定的顺序呈现它们，可使用函数sorted()



* 要反转列表元素的排列顺序，可使用方法reverse()

reverse()不是按与字母顺序相反的顺序排列列表元素，而只是反转列表元素的排列顺序



使用函数len()可快速获悉列表的长度

发生索引错误却找不到解决办法时，请尝试将列表或其长度打印出来



# 第四章   操作列表

## 4.1 遍历整个列表

需要对列表中的每个元素都执行相同的操作时，可使用Python中的for循环

```python
for bicycle in bicycles:
	print(bicycle)
```



在for循环后面，没有缩进的代码都只执行一次，不会重复执行



## 4.2 避免缩进错误

Python根据缩进来判断代码行与前一个代码行的关系



较为常见的缩进错误：

* 忘记缩进：对于位于for语句后面且属于循环组成部分的代码行，一定要缩进
* 忘记缩进额外的代码行：试图在循环中执行多项任务，却忘记缩进其中的一些代码行时，就会出现这种情况
* 不必要的缩进：如果你不小心缩进了无须缩进的代码行，Python将指出这一点
* 循环后不必要的缩进：如果不小心缩进了应在循环结束后执行的代码，这些代码将针对每个列表元素重复执行
* 遗漏了冒号：for语句末尾的冒号告诉Python，下一行是循环的第一行



## 4.3 创建数值列表

列表非常适合用于存储数字集合，而Python提供了很多工具，可帮助你高效地处理数字列表



1. **函数range()**

Python函数range()让你能够轻松地生成一系列数

```python
# 打印1~5
for value in range(1,5):
    print(value)
```



2. **使用range()创建数字列表**

要创建数字列表，可使用函数list()将range()的结果直接转换为列表

```python
numbers = list(range(1,6))

# 使用第三个参数指定步长
even_numbers = list(range(2, 11, 2))
```



3. **对数字列表执行简单的统计计算**

```python
min(numbers) # 求列表最小值
max(numbers) # 求列表最大值
sum(numbers) # 求列表的和
```



4. **列表解析**

列表解析将for循环和创建新元素的代码合并成一行，并自动附加新元素

```python
# 得到1~10的平方数
squares = [value ** 2 for value in range(1, 11)]
```



## 4.4 使用列表的一部分

可以处理列表的部分元素，Python称之为切片



1. **切片**

要创建切片，可指定要使用的第一个元素和最后一个元素的索引

```python
# 打印前三个
print(bicycles[0:3])
```



如果没有指定第一个索引，Python将自动从列表开头开始

要让切片终止于列表末尾，也可使用类似的语法

```python
print(bicycles[:4])
print(bicycles[2:])
```



2. **遍历切片**

如果要遍历列表的部分元素，可在for循环中使用切片

```python
for bicycle in bicycles[:3]:
    print(bicycle.title())
```



3. **复制列表**

要复制列表，可创建一个包含整个列表的切片，方法是同时省略起始索引和终止索引（[:]）



## 4.5 元组

Python将不能修改的值称为不可变的，而不可变的列表被称为元组



1. **定义元组**

元组看起来很像列表，但使用圆括号而非中括号来标识

定义元组后，就可使用索引来访问其元素，就像访问列表元素一样

```python
dimensions = (200, 50)
print(dimensions[0])

dimensions[0] = 250 # 元组元素无法修改，报错
```



2. **修改元组变量**

虽然不能修改元组的元素，但可以给存储元组的变量赋值

```pyth
dimensions = (200, 50)
dimensions = (400, 100)
```



## 4.6 设置代码格式

* 缩进：PEP 8建议每级缩进都使用四个空格
* 行长：建议每行不超过80字符，注释的行长不应超过72字符
* 空行：要将程序的不同部分分开，可使用空行，但也不能滥用



# 第五章   if语句

要判断特定的值是否已包含在列表中，可使用关键字in

确定特定的值未包含在列表中很重要。在这种情况下，可使用关键字not in

```python
'trek' in bicycles
'trek' not in bicycles
```



最简单if语句：

```py
if condition:
	do something
```



if语句中，缩进的作用与在for循环中相同

如果测试通过了，将执行if语句后面所有缩进的代码行，否则将忽略它们



if-elif-else结构



在运行for循环前确定列表是否为空



# 第六章   字典

## 6.1 一个简单的字典

```python
alien = {'color':'green', 'points':5}
print(alien['color'])
```



## 6.2 使用字典

字典是一系列键值对

每个键都与一个值相关联，你可使用键来访问相关联的值

可将任何Python对象用作字典中的值



键值对是两个相关联的值

指定键时，Python将返回与之相关联的值



1. **访问字典中的值**

要获取与键相关联的值，可依次指定字典名和放在方括号内的键

字典中可包含任意数量的键值对



2. **添加键值对**

```python
alien['x_position'] = 0
alien['y_position'] = 25
```



3. **创建空字典**

```python
alien = {}
```



4. **修改字典中的值**

```python
alien['color'] = 'yellow'
```



5. **删除键值对**

对于字典中不再需要的信息，可使用del语句将相应的键值对彻底删除

```python
del alien['points']
```



6. **使用get()来访问值**

使用放在方括号内的键从字典中获取感兴趣的值时，可能会引发问题：如果指定的键不存在就会出错

字典可使用方法get()在指定的键不存在时返回一个默认值，从而避免这样的错误



方法get()的第一个参数用于指定键，是必不可少的；第二个参数为指定的键不存在时要返回的值，是可选的

```python
point_value = alien.get('points', 'No point value assign')
```



## 6.3 遍历字典

1. **遍历所有键值对**

```python
for key,value in alien.items():
    print(f"\nKey: {key}")
    print(f"Value: {value}")
```

key和value可以用其他变量名代替



items()返回一个键值对列表



2. **遍历字典中的所有键**

```python
for key in alien.keys():
    print(key)
```

方法keys()并非只能用于遍历：实际上，它返回一个列表，其中包含字典中的所有键



3. **按特定顺序遍历字典中的所有键**

从Python 3.7起，遍历字典时将按插入的顺序返回其中的元素

为此，可使用函数sorted()来获得按特定顺序排列的键列表的副本



4. **遍历字典中的所有值**

如果主要对字典包含的值感兴趣，可使用方法values()来返回一个值列表，不包含任何键



为剔除重复项，可使用集合（set）。集合中的每个元素都必须是独一无二的

```python
for value in set(alien.values()):
    print(value)
```



## 6.4 嵌套

有时候，需要将一系列字典存储在列表中，或将列表作为值存储在字典中，这称为嵌套



1. **字典列表**

```python
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}

aliens = [alien_0, alien_1, alien_2]

for alien in aliens:
      print(alien)
```



2. **在字典中存储列表**

有时候，需要将列表存储在字典中，而不是将字典存储在列表中

```python
  # 存储所点比萨的信息。
 pizza = {
      'crust': 'thick',
      'toppings': ['mushrooms', 'extra cheese'],
      }

  # 概述所点的比萨。
 print(f"You ordered a {pizza['crust']}-crust pizza "
      "with the following toppings:")

 for topping in pizza['toppings']:
      print("\"f+topping)
```



每当需要在字典中将一个键关联到多个值时，都可以在字典中嵌套一个列表









