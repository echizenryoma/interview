## 强制类型转换

1. `static_cast`主要用于非多态类型之间的转换，不提供运行时的检查来确保安全的检查
2. `dynamic_cast`只能转换指针类型和引用类型，不能转换其他类型
3. `const_cast`主要是用来修改的`const`和`volatile`属性。
4. `reinterpret_cast`
    1. 用在任意指针（或引用）类型之间的转换
    2. 指针与足够大的整数类型之间的转换
    3. 从整数类型到指针类型

