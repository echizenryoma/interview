## 多态

C++的多态性是通过虚函数来实现的，虚函数允许子类重新定义成员函数，而子类重新定义父类的做法称为覆盖。多态与非多态的实质区别就是函数的地址是**运行时**确定还是**编译时**确定。如果函数的调用在编译器编译期间就可以确定函数的调用地址，并生产代码，是静态的。而如果函数调用的地址在运行时才确定，就是动态的。

### 虚表

类`A`包含虚函数`vfunc1`、`vfunc2`，由于类`A`包含虚函数，故类`A`拥有一个虚表。

```cpp
class A {
public:
    virtual void vfunc1();
    virtual void vfunc2();
    void func1();
    void func2();
private:
    int m_data1, m_data2;
};
```
![](/assets/cpp-virtual-table.svg)

虚表是一个指针数组，其元素是虚函数的指针，每个元素对应一个虚函数的函数指针。需要指出的是，普通的函数即非虚函数，其调用并不需要经过虚表，所以虚表的元素并不包括普通函数的函数指针。

虚表内的条目，即虚函数指针的赋值发生在编译器的编译阶段，也就是说在代码的编译阶段，虚表就可以构造出来了。

### 虚表指针

虚表是属于类的，而不是属于某个具体的对象，一个类只需要一个虚表即可。同一个类的所有对象都使用同一个虚表。 

为了指定对象的虚表，对象内部包含一个虚表的指针，来指向自己所使用的虚表。为了让每个包含虚表的类的对象都拥有一个虚表指针，编译器在类中添加了一个指针`*__vptr`用来指向虚表。这样，当类的对象在创建时便拥有了这个指针，且这个指针的值会自动被设置为指向类的虚表。

![](/assets/cpp-virtual-table-pointer.svg)

一个继承类的基类如果包含虚函数，那个这个继承类也有拥有自己的虚表，故这个继承类的对象也包含一个虚表指针，用来指向它的虚表。


### 动态绑定

动态绑定最常见的用法就是声明基类的指针，利用该指针指向任意一个子类对象，调用相应的虚函数，可以根据指向的子类的不同而实现不同的方法。

类A是基类，类B继承类A，类C又继承类B。

```cpp
class A {
public:
    virtual void vfunc1();
    virtual void vfunc2();
    void func1();
    void func2();
private:
    int m_data1, m_data2;
};

class B : public A {
public:
    virtual void vfunc1();
    void func1();
private:
    int m_data3;
};

class C: public B {
public:
    virtual void vfunc2();
    void func2();
private:
    int m_data1, m_data4;
};
```

![](/assets/cpp-dynamic-binding.svg)

#### 动态绑定的必要条件

1. 通过指针来调用函数
2. 指针upcast向上转型
3. 调用的是虚函数
