## dataclass装饰器

先看一段代码
```python
from dataclasses import dataclass
@dataclass
class Inventory_item:
  name:str
  number:int
  value:float
  def multi(self) -> float:
    return self.number * self.value
```
```python
class Inventory_item:
  def __init__(self,name:str,number:int ,value:float):
    self.name=name
    self.number=number
    self.value=value
```
### 
- init默认为真值，会生成__init__方法，若类已经定义则删去默认生成
- repr默认为真值，会生成__repr__方法（有类名，每个字段的名称及repr，字段按照类中生成顺序排序），若类已经定义则删去默认生成。repr用于开发者。
- eq默认为真值，会生成__eq__方法，把类当作名称生成的元组进行比较
- order: 如为真值 (默认为 False)，将生成 __lt__(), __le__(), __gt__() 和 __ge__() 方法。
  这些方法将把类当作由其字段组成的元组那样按顺序进行比较。 要比较的两个实例必须是相同的类型.。若order为True而eq为False，返回ValueError
- hash
unsafe_hash：默认关闭，强制生成哈希方法（用于字典/集合），但可能不安全。

- 哈希值生成规则：
  - 哈希安全性：dataclass默认不生成哈希方法，除非类属性不可变（安全）。
  - 可变对象：如果类属性能改，哈希值可能不准，所以默认不生成。
  - eq影响：如果开启了eq比较，dataclass会根据情况决定是否生成哈希方法。
  - [详细参考](https://docs.python.org/zh-cn/3/library/dataclasses.html)
- 高级参数控制
  - frozen：默认关闭，开启后对象属性不能修改（类似只读）。
  - match_args：默认开启，自动生成匹配参数的元组。
  - kw_only：默认关闭，开启后字段必须用关键字参数传入。
  - slots：默认关闭，开启后优化内存使用，但可能影响继承。
  
