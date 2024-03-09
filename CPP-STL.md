# C++ STL

## Vector

### 初始化

```cpp
#include <vector>

std::vector<typename _Tp, typename _Alloc = std::allocator<_Tp> >();
```

第一个是重要的参数, 代表动态数组中的值的类型.

### 常用方法

获取动态数组长度.

```cpp
size_type size() const;
```

在数组列表最后插入数据.

```cpp
void push_back( const T& value );
```

## String

### 初始化

```cpp
std::string();
```

### 常用方法

获取字符串长度.

```cpp
// 二者相同
size_t length() const noexcept;
size_t size() const noexcept;
```

获取字符串子串.

```cpp
basic_string substr( size_type pos = 0, size_type count = npos ) const;
```

查找方法, 从pos开时查找str, 返回第一次找到的位置.

```cpp
size_type find(const string& str, size_type pos) const; 
```

修改字符串字符数.

```cpp
void resize( size_type count );
```

字符串替代, 将[first, last)替换为str, 返回替换后的字符串.

```cpp
basic_string& replace( const_iterator first, const_iterator last, const basic_string& str );
```

字符串插入, 在pos前插入[first, last), 返回插入后的迭代器.

```cpp
template< class InputIt >
iterator insert( const_iterator pos, InputIt first, InputIt last );
```

字符串删除, 删除[first, last)的元素, 返回last迭代器.

```cpp
iterator erase( const_iterator first, const_iterator last );
```

## Stack

### 初始化

```cpp
std::stack<class T> ();
```

### 常用函数

判断栈是否为空.

```cpp
bool empty();
```

入栈.

```cpp
void push(const value_type& x);
```

出栈.

```cpp
void pop();
```

取栈顶元素.

```cpp
value_type& top();
```

## HashMap

### 初始化

```cpp
#include <hash_map>

std::hash_map<class _Key, class _Tp, class _HashFcn = hash<_Key>,
class _EqualKey = equal_to<_Key>,
class _Alloc = __STL_DEFAULT_ALLOCATOR(_Tp) > hash_map_name();
```

从上面可以看出, 重要的模板参数为前四个, 第一个是key, 第二个是value, 第三个是hash函数, 第四个是比较函数. 构造函数中的参数用来设置hash表的大小. 下面是hash函数和比较函数的例子, 可依此自定义这两个函数(自定义类作为类型的时候).

```cpp
//hash函数, 自定义时不需要必定为模板函数, 重载结构体的operator()为返回值为size_t类型的常函数即可.
struct hash<int>
{
    size_t operator()(int __x) const 
    { 
        return __x;
    }
};

//比较函数, 自定义时不需要必定为模板函数, 重载结构体的operator()为返回值为bool类型的常函数即可.
template <class _Tp>
struct equal_to : public binary_function<_Tp,_Tp,bool>
{
    bool operator()(const _Tp& __x, const _Tp& __y) const 
    {
        return __x == __y;
    }
};

//比较函数结构体继承的结构体(定义了三个类型的别名)
template <class _Arg1, class _Arg2, class _Result>
struct binary_function {
        typedef _Arg1 first_argument_type;
        typedef _Arg2 second_argument_type;
        typedef _Result result_type;
};
```

hash_map在C11中加入STL模板库中, 但为了与以前程序员自己用的hash_map区分使用的是备用名unordered_map.
