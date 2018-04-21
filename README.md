![cover](https://img11.360buyimg.com/n1/jfs/t18112/283/105345917/276856/cfc529d5/5a5cd735Naa769cef.jpg)

# 类型

任何实例都有id值，不论是字面量还是变量
```
>>> a = 1
>>> id(a)
1673948176
>>> id(123)
1673952080
```

`type`返回实例类型
```
>>> type(1)
<class 'int'>
```

`isinstance`判断实例类型
```
>>> isinstance(1, float)
False
```

`issubclass`判断继承关系
```
>>> class Animal:pass

>>> class Mammal(Animal):pass

>>> class Lion(Mammal):pass

>>> issubclass(Lion, Mammal)
True
>>> lion = Lion()
>>> isinstance(lion, Lion)
True
>>> isinstance(lion, Mammal)
True
>>> issubclass(Lion, Animal)
True
```

所有类型的共同祖先是`object`
```
>>> issubclass(Animal, object)
True
```

类型也是普通的对象实例，由`type`创建
```
>>> id(int)
1673501152
>>> type(int)
<class 'type'>
>>> isinstance(int, type)
True
```

在解释型动态语言里，名字和对象通常是两个运行期实体，名字不但有自己的类型，还需分配内存，这点和静态编译语言不同。

## namespace

`namespace`是上下文环境里专门用来存储名字和目标引用关联的容器。对Python而言，每个模块（源码文件）有一个全局`namespace`。而根据代码作用域，又有当前`namespace`或本地`namespace`。如果直接在模块级别执行，那么当前`namespace`和全局`namespace`相同。但在函数内，当前`namespace`就专指函数作用域

>这段话回头再来理解，先照抄

`namespace`默认使用`dict`数据结构，`globals()`和`locals()`用来返回全局`namespace`和本地`namespace`的`dict`对象
```
>>> id(globals())
2007044346360
>>> id(locals())
2007044346360
>>> #在模块级别，两者相同
>>> globals()
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'Animal': <class '__main__.Animal'>, 'Mammal': <class '__main__.Mammal'>, 'Lion': <class '__main__.Lion'>, 'lion': <__main__.Lion object at 0x000001D34F9EBBE0>}
>>> locals()
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'Animal': <class '__main__.Animal'>, 'Mammal': <class '__main__.Mammal'>, 'Lion': <class '__main__.Lion'>, 'lion': <__main__.Lion object at 0x000001D34F9EBBE0>}
>>> def test():
	x = "hello"
	print(locals())
	print("locals: ", id(locals()))
	print("globals:", id(globals()))

	
>>> test()
{'x': 'hello'}
locals:  2007085045152
globals: 2007044346360
>>> #在函数作用域，两者不同
```

只要乐意，直接修改`namespace`建立关联引用都可以
```
>>> globals()["hello"] = "world"
>>> hello
'world'
```

赋值操作是让名字在`namespace`里重新关联，而非修改原对象  
>想象成所有值都是引用型，以下代码来证明
```
>>> x = 1234
>>> y = x
>>> id(x)
2007085731920
>>> id(y)
2007085731920
>>> a = 1234
>>> b = 1234
>>> id(a)
2007085732336
>>> id(b)
2007085732080
```

## 命名规则

* 字母下划线开头
* 区分大小写
* 不能用关键字

检查关键字
```
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
>>> keyword.iskeyword("is")
True
```

下划线的特殊含义

* 单下划线_x属于私有成员，不会被导入
* 双下划线开头__x属于自动重命名私有成员
* 双下划线开头结尾__x__属于系统成员，避免使用
* 交互模式下，单下划线返回最后一个表达书的结果

## 内存
