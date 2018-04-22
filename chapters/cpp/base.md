## C++基础

### static

1. `全局静态变量`：只在当前文件中可见（`internal linkage`）
2. `局部静态变量`：只在此函数内可见（同时，在多次函数调用中，变量的值不会丢失）
3. `静态函数`：只在当前文件中可见（`internal linkage`）
4. `类静态成员变量`和`类静态成员函数`：只在此类中可见

#### 初始化静态成员变量

```cpp
class A
{  
public:  
    static int a;
    A(){}
};

int A::a = 7;
```

```cpp
class A
{  
public:  
    const static int a = 7;
    A(){}  
};
```

### extern "C"



