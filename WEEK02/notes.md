# 《fluent python》
-----
## 零散知识点
### ！r   !s 
在 Python 中，self.x!r 里的 !r 是 格式化字符串（f-string 或 str.format()）中的格式说明符，用于指定对象 x 的“表示形式”。
具体来说：

!r 调用对象的 __repr__() 方法，返回“官方/开发者友好的字符串表示”（通常包含类型和内部状态，便于调试）。
对比：!s 调用 __str__()（用户友好的字符串表示），!a 调用 __ascii__()（ASCII 兼容的表示）。
```python
class MyClass:
    def __init__(self, value):
        self.value = value
    def __repr__(self):
        return f"MyClass(value={self.value})"  # 开发者友好的表示

obj = MyClass(42)
print(f"{obj!r}")  # 输出: MyClass(value=42)
print(f"{obj!s}")  # 输出: 42（如果 __str__ 未定义，默认行为）
```
!r 常用于调试或日志，因为它能更清晰地展示对象的内部状态（比如类型、属性值），帮助开发者快速定位问题

### !r   %r
- %r：旧式字符串格式化（% 操作符）中的占位符，调用 repr()。
- !r：新式字符串格式化（str.format() 和 f-string）中的转换字段，同样调用 repr()。

本质相同：两者都调用 repr() 来获取对象的“官方”字符串表示，常用于调试。
```python
name='python'
print('Hello,%r'%name)

name='python'
print('Hello,{name!r}')

name='pyhton'
print('Hello,{!r}'.format(name))
```

### ord() 和 chr()
ord()返回Unicode码，chr互逆
### 海象作用域
海象运算符 := 是赋值操作，在循环中每轮都会执行并覆盖，只能得到最后一个值。
```python
x='ABC'
code=[ord(x) for x in x]
print(x)    #'ABC'
print(code) #[65, 66, 67]
codes=[last:=ord(c) for c in x]
prnt(codes)   #67```

print([ord()])
```
### 函数表
| Category | Method names |
| --- | --- |
| String/bytes representation | `__repr__` `__str__` `__format__` `__bytes__` `__fspath__` |
| Conversion to number | `__bool__` `__complex__` `__int__` `__float__` `__hash__` `__index__` |
| Emulating collections | `__len__` `__getitem__` `__setitem__` `__delitem__` `__contains__` |
| Iteration | `__iter__` `__aiter__` `__next__` `__anext__` `__reversed__` |
| Callable or coroutine execution | `__call__` `__await__` |
| Context management | `__enter__` `__exit__` `__aexit__` `__aenter__` |
| Instance creation and destruction | `__new__` `__init__` `__del__` |
| Attribute management | `__getattr__` `__getattribute__` `__setattr__` `__delattr__` `__dir__` |
| Attribute descriptors | `__get__` `__set__` `__delete__` `__set_name__` |
| Abstract base classes | `__instancecheck__` `__subclasscheck__` |
| Class metaprogramming | `__prepare__` `__init_subclass__` `__class_getitem__` `__mro_entries__` |

### why len is not a method

len不被调用为方法是因为它在Python数据模型中得到了特殊对待，就像abs一样。这种设计使得len在处理内置类型（如字符串、列表等）时非常高效，因为它可以直接从C结构体中读取长度，而不需要调用任何方法。此外，通过特殊方法__len__，用户也可以使len与自己自定义的对象一起工作，这既保证了内置对象的效率，又保持了语言的一致性.

-----
## array of sequence
### built-in sequence
- 容器序列 container sequence :
可以存放不同类型的数据项，包括嵌套容器。例如：`list`、`tuple` 和 `collections.deque`。

- 扁平序列 flat sequence :
存放一种简单类型的数据项。例如：`str`、`bytes` 和 `array.array`。



- 可变序列可以原地修改内容，不可变序列一旦创建就不能更改。

内存层面
不可变：修改会创建新对象，id 变化

可变：修改不创建新对象，id 不变

方法层面
不可变（Sequence ABC）：只有读方法（__getitem__、count、index等）

可变（MutableSequence ABC）：继承不可变所有方法，额外实现写方法（__setitem__、append、pop、extend等）

### 2.6拆包
#### 下划线
#### 星号
#### 模式匹配
```python
for data in ["python",3.14,[10,20,30]]:
    match data:
        case str():print(f"文本：{data}")
        case int() | float():print(f"数值：{data}")
        case [first,*rest]:print(f"列表首项：{first}")
##文本：python
##数值：3.14
##列表首项：10
```
match 关键字后面的表达式是匹配对象（subject），即各个 case句中的模式尝试匹配的数据
##### 析构 destructuring
不仅可以判断值是否相等，还可以判断数据结构是否吻合，吻合的话提取出来。
```python
metro_areas = [
 ('Tokyo', 'JP', 36.933, (35.689722, 139.691667)),
 ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
 ('Mexico City', 'MX', 20.142, (19.433333, -99.133333)),
 ('New York-Newark', 'US', 20.104, (40.808611, -74.020386)),
 ('São Paulo', 'BR', 19.649, (-23.547778, -46.635833)),
]
def main():
    print(f:"{"":15} | {"latitude":>9} | {"longititude":>9}")
    for record in metro_area:
        case record:
            case [name,_,_,[lat,lon]] if lon<0: print(f"{"name":15} | {"lat":9.4f} | {"lon":9.4f}")
```
15：表示预留 15 个字符的宽度。这里是在格式化一个空字符串 ""，默认是左对齐  
大于9：9 表示预留 9 个字符的宽度。> 是一个对齐符号，表示右对齐（Right-aligned）

一般来说，匹配对象同时满足以下条件方能匹配序列模式：
- 是序列
- 和模式匹配项数相等
- 对应项相互匹配，包括嵌套的项

  
