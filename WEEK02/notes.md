# 《fluent python》
## 零散知识点
- ！r !s 
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
