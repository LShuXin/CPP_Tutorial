> C++ 类构造函数 & 析构函数

# 菜鸟教程

## 类的构造函数

类的**构造函数**是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。

下面的实例有助于更好地理解构造函数的概念：

## 实例

```
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength(double len);
      double getLength(void);
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength(double len)
{
    length = len;
}
 
double Line::getLength(void)
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```



当上面的代码被编译和执行时，它会产生下列结果：

```
Object is being created
Length of line : 6
```

## 带参数的构造函数

默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象赋初始值，如下面的例子所示：

## 实例

```
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength(double len);
      double getLength(void);
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
 
void Line::setLength(double len)
{
    length = len;
}
 
double Line::getLength(void)
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
 
   // 获取默认设置的长度
   cout << "Length of line : " << line.getLength() <<endl;
   // 再次设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```



当上面的代码被编译和执行时，它会产生下列结果：

```
Object is being created, length = 10
Length of line : 10
Length of line : 6
```

## 使用初始化列表来初始化字段

使用初始化列表来初始化字段：

```
Line::Line(double len): length(len) {    
    cout << "Object is being created, length = " << len << endl; 
}
```

上面的语法等同于如下语法：

```
Line::Line(double len) {    
  length = len;    
  cout << "Object is being created, length = " << len << endl;
}
```



假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：

```
C::C(double a, double b, double c): X(a), Y(b), Z(c) {  .... }
```



## 类的析构函数

类的**析构函数**是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。

析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

下面的实例有助于更好地理解析构函数的概念：

## 实例

```
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength(double len);
      double getLength(void);
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
}
 
void Line::setLength(double len)
{
    length = len;
}
 
double Line::getLength(void)
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```



当上面的代码被编译和执行时，它会产生下列结果：

```
Object is being created
Length of line : 6
Object is being deleted
```



# cbiancheng

能出现在赋值号左边的表达式称为“左值”，不能出现在赋值号左边(只能出现在赋值号右边)的表达式称为“右值”。一般来说，左值是可以取地址的，右值则不可以。

**非 const 的变量都是左值。函数调用的返回值若不是引用，则该函数调用就是右值？？。前面所学的“引用”都是引用变量的，而变量是左值，因此它们都是“左值引用”（左值引用就是引用变量）**。

[C++](http://c.biancheng.net/cplus/)11 新增了一种引用，可以引用右值，因而称为“右值引用”。无名的临时变量不能出现在赋值号左边，因而是右值。右值引用就可以引用无名的临时变量。定义右值引用的格式如下：

类型 && 引用名 = 右值表达式;

例如：

```
class A{};
A &rl = A();    // 错误，无名临时变量 A() 是右值（这里所指的无名临时变量十类的实例），因此不能初始化左值引用r1

A &&r2 = A();  // 正确，因 r2 是右值（类的实例对象只能被赋值给其他变量，所以是右值）引用
```


引入右值引用的主要目的是提高程序运行的效率。有些对象在复制时需要进行深复制，深复制往往非常耗时。合理使用右值引用可以避免没有必要的深复制操作。例如下面的程序：

```
#include <iostream>
#include <string>
#include <cstring>
using namespace std;

class String
{
public:
    char* str;
    
    //默认构造函数
    String() : str(new char[1]) {
      str[0] = 0; 
    }
    
    //拷贝构造函数（指针形式）
    String(const char* s) {
        str = new char[strlen(s) + 1];
        strcpy(str, s);
    }
    
    //拷贝构造函数（引用形式）
    String(const String & s) {
        cout << "copy constructor called" << endl;
        str = new char[strlen(s.str) + 1];
        strcpy(str, s.str);
    }
    
    //复制赋值号
    //下文中（所谓的19行）
    String & operator = (const String & s) {
        cout << "copy operator = called" << endl;
        if (str != s.str) {
            delete[] str;
            str = new char[strlen(s.str) + 1];
            strcpy(str, s.str);
        }
        return *this;
    }
    
    //移动构造函数（注意这里的构造函数含有一个右值参数）
    String(String && s) : str(s.str) {
        
        //上面的初始化列表将右值直接赋值给了类的成员变量
        cout << "move constructor called" << endl;
        
        //将原来的右值内容清除
        s.str = new char[1];
        s.str[0] = 0;
    }
    
    //移动赋值号
    //下文中所谓的33行
    String & operator = (String && s) {
        cout << "move operator = called" << endl;
        if (str != s.str) {
            str = s.str;
            
            //将原来的右值内容清除
            s.str = new char[1];
            s.str[0] = 0;
        }
        return *this;
    }
    ~String() { delete[] str; }
};

template <class T>
void MoveSwap(T & a, T & b) {
    T tmp(move(a));  //std::move(a) 为右值，这里（tmp初始化的过程）会调用移动构造函数
    a = move(b);     //move(b) 为右值，因此这里会调用移动赋值号
    b = move(tmp);   //move(tmp) 为右值，因此这里会调用移动赋值号
}
int main()
{
    String s;
    s = String("this");  //这里的赋值操作将调用移动赋值号，因为参数是右值
    cout << "* * * *" << endl;
    cout << s.str << endl;
    
    String s1 = "hello", s2 = "world";
    MoveSwap(s1, s2);  //调用一次移动构造函数和两次移动赋值号
    cout << s2.str << endl;
    
    return 0;
}
```

程序的输出结果如下：
```
move operator = called
****
this
move constructor called
move operator = called
move operator = called
hello
```



第 33 行重载了一个移动赋值号。它和第 19 行的复制赋值号的区别在于，其参数是右值引用。在移动赋值号函数中没有执行深复制操作，而是直接将对象的 str 指向了参数 s 的成员变量 str 指向的地方，然后修改 s.str 让它指向别处，以免 s.str 原来指向的空间被释放两次。

该移动赋值号函数修改了参数，这会不会带来麻烦呢？答案是不会。因为移动赋值号函数的形参是一个右值引用，则调用该函数时，实参一定是右值。右值一般是无名临时变量，而无名临时变量在使用它的语句结束后就不再有用，因此其值即使被修改也没有关系。

第 53 行，如果没有定义移动赋值号，则会导致复制赋值号被调用，引发深复制操作。临时无名变量`String("this")`是右值，因此在定义了移动赋值号的情况下，会导致移动赋值号被调用。移动赋值号使得 s 的内容和 String("this") 一致，然而却不用执行深复制操作，因而效率比复制赋值号高。

虽然移动赋值号修改了临时变量 String("this")，但该变量在后面已无用处，因此这样的修改不会导致错误。

第 46 行使用了 C++ 11 中的标准模板 move。move 能接受一个左值作为参数，返回该左值的右值引用。因此本行会用定义于第 28 行、以右值引用作为参数的移动构造函数来初始化 tmp。该移动构造函数没有执行深复制，将 tmp 的内容变成和 a 相同，然后修改 a。由于调用 MoveSwap 本来就会修改 a，所以 a 的值在此处被修改不会产生问题。

第 47 行和第 48 行调用了移动赋值号，在没有进行深复制的情况下完成了 a 和 b 内容的互换。对比 Swap 函数的以下写法：

```
template <class T>
void Swap(T & a, T & b) {    
  T tmp(a);  //调用复制构造函数    
  a = b;     //调用复制赋值号   
  b = tmp;   //调用复制赋值号
}
```

Swap 函数执行期间会调用一次复制构造函数，两次复制赋值号，即一共会进行三次深复制操作。而利用右值引用，使用 MoveSwap，则可以在无须进行深复制的情况下达到相同的目的，从而提高了程序的运行效率。



# learn.microsoft.com

若要自定义类初始化其成员的方式，或是如要在创建类的对象时调用函数，请定义构造函数。 构造函数具有与类相同的名称，没有返回值。 可以定义所需数量的重载构造函数，以各种方式自定义初始化。 通常，构造函数具有公共可访问性，以便类定义或继承层次结构外部的代码可以创建类的对象。 但也可以将构造函数声明为 **`protected`** 或 **`private`**。

构造函数可以选择采用成员初始化表达式列表。 与在构造函数体中赋值相比，初始化类成员是**更高效**的方式。 以下示例演示具有三个重载构造函数的类 `Box`。 最后两个构造函数使用成员初始化列表：

C++复制

```cpp
class Box {
public:
    // Default constructor
    Box() {}

    // Initialize a Box with equal dimensions (i.e. a cube)
    explicit Box(int i) : m_width(i), m_length(i), m_height(i) // member init list
    {}

    // Initialize a Box with custom dimensions
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}

    int Volume() { return m_width * m_length * m_height; }

private:
    // Will have value of 0 when default constructor is called.
    // If we didn't zero-init here, default constructor would
    // leave them uninitialized with garbage values.
    int m_width{ 0 };
    int m_length{ 0 };
    int m_height{ 0 };
};
```

声明类的实例时，编译器会基于重载决策选择要调用的构造函数：

C++复制

```cpp
int main()
{
    Box b; // Calls Box()

    // Using uniform initialization (preferred):
    Box b2 {5}; // Calls Box(int)
    Box b3 {5, 8, 12}; // Calls Box(int, int, int)

    // Using function-style notation:
    Box b4(2, 4, 6); // Calls Box(int, int, int)
}
```

- 构造函数可以声明为 **`inline`**、[`explicit`](https://learn.microsoft.com/zh-cn/cpp/cpp/constructors-cpp?view=msvc-170#explicit_constructors)、[`friend`](https://learn.microsoft.com/zh-cn/cpp/cpp/friend-cpp?view=msvc-170) 或 [`constexpr`](https://learn.microsoft.com/zh-cn/cpp/cpp/constructors-cpp?view=msvc-170#constexpr_constructors)。
- 构造函数可以初始化一个已声明为 **`const`**、**`volatile`** 或 **`const volatile`** 的对象。 该对象在构造函数完成之后成为 **`const`**。
- 若要在实现文件中定义构造函数，请为它提供限定名称，如同任何其他成员函数一样：`Box::Box(){...}`。



## 成员初始化表达式列表

构造函数可以选择具有成员初始化表达式列表，该列表会在**构造函数主体运行之前**初始化类成员。 **（成员初始化表达式列表与类型为 [`std::initializer_list`](https://learn.microsoft.com/zh-cn/cpp/standard-library/initializer-list-class?view=msvc-170) 的初始化表达式列表不同。）**

首选成员初始化表达式列表，而不是在构造函数主体中赋值。 成员初始化表达式列表直接初始化成员。 以下示例演示了成员初始化表达式列表，该列表由冒号后的所有 `identifier(argument)` 表达式组成：

C++复制

```cpp
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}
```

标识符必须引用类成员；它使用参数的值进行初始化。 参数可以是构造函数参数之一、函数调用或 [`std::initializer_list`](https://learn.microsoft.com/zh-cn/cpp/standard-library/initializer-list-class?view=msvc-170)。

‼️**`const`** 成员和引用类型的成员必须在成员初始化表达式列表中进行初始化。

‼️若要确保在派生构造函数运行之前完全初始化基类，请调用初始化表达式列表中的任何参数化基类构造函数。



## 默认构造函数

默认构造函数通常没有参数，但它们可以具有带默认值的参数。

C++复制

```cpp
class Box {
public:
    Box() { /*perform any required default initialization steps*/}

    // All params have default values
    Box (int w = 1, int l = 1, int h = 1): m_width(w), m_height(h), m_length(l){}
...
}
```

默认构造函数是[特殊成员函数](https://learn.microsoft.com/zh-cn/cpp/cpp/special-member-functions?view=msvc-170)之一。 如果类中未声明构造函数，则编译器提供隐式 **`inline`** 默认构造函数。

C++复制

```cpp
#include <iostream>
using namespace std;

class Box {
public:
    int Volume() {return m_width * m_height * m_length;}
private:
    int m_width { 0 };
    int m_height { 0 };
    int m_length { 0 };
};

int main() {
    Box box1; // Invoke compiler-generated constructor
    cout << "box1.Volume: " << box1.Volume() << endl; // Outputs 0
}
```

如果你依赖于隐式默认构造函数，请确保在类定义中初始化成员，如前面的示例所示。 如果没有这些初始化表达式，成员会处于未初始化状态，Volume() 调用会生成垃圾值。 一般而言，即使不依赖于隐式默认构造函数，也最好以这种方式初始化成员。

可以通过将隐式默认构造函数定义为[已删除](https://learn.microsoft.com/zh-cn/cpp/cpp/constructors-cpp?view=msvc-170#explicitly_defaulted_and_deleted_constructors)来阻止编译器生成它：

C++复制

```cpp
    // Default constructor
    Box() = delete;
```

如果有任何类成员不是默认可构造，则编译器生成的默认构造函数会定义为已删除。 例如，类类型的所有成员及其类类型成员都必须具有可访问的默认构造函数和析构函数。 引用类型的所有数据成员和所有 **`const`** 成员都必须具有默认成员初始化表达式。

调用编译器生成的默认构造函数并尝试使用括号时，系统会发出警告：

C++复制

```cpp
class myclass{};
int main(){
    myclass mc();     // warning C4930: prototyped function not called (was a variable     
    definition intended?)
}
```

此语句是“最棘手的分析”问题的示例。 可以将 `myclass md();` 解释为函数声明或是对默认构造函数的调用。 因为 C++ 分析程序更偏向于声明，因此表达式会被视为函数声明。 有关详细信息，请参阅[最棘手的分析](https://en.wikipedia.org/wiki/Most_vexing_parse)。

**如果声明了任何非默认构造函数，编译器不会提供默认构造函数：**

C++复制

```cpp
class Box {
public:
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height){}
private:
    int m_width;
    int m_length;
    int m_height;

};

int main(){

    Box box1(1, 2, 3); // 圆括号
    Box box2{2, 3, 4}; // 大括号
    Box box3; // C2512: no appropriate default constructor available
}
```

**如果类没有默认构造函数，则无法通过单独使用方括号语法来构造该类的对象数组。 例如，在前面提到的代码块中，Box 数组无法进行如下声明：**

C++复制

```cpp
Box boxes[3]; // C2512: no appropriate default constructor available
```

**但是，你可以使用一组初始化表达式列表来初始化 Box 对象数组：**

C++复制

```cpp
Box boxes[3]{ { 1, 2, 3 }, { 4, 5, 6 }, { 7, 8, 9 } };
```

有关详细信息，请参阅[初始化表达式](https://learn.microsoft.com/zh-cn/cpp/cpp/initializers?view=msvc-170)。



## 复制构造函数

复制构造函数通过从相同类型的对象**复制成员值**来初始化对象。 如果类成员都是简单类型（如标量值），则编译器生成的复制构造函数已够用，你无需定义自己的函数。 如果类需要更复杂的初始化，则需要实现自定义复制构造函数。 例如，如果类成员是指针，则需要定义复制构造函数以分配新内存，并从其他指针指向的对象复制值。 编译器生成的复制构造函数只是复制指针，以便新指针仍指向其他指针的内存位置。

复制构造函数可能具有以下签名之一：

C++复制

```cpp
    Box(Box& other); // Avoid if possible--allows modification of other.
    Box(const Box& other);
    Box(volatile Box& other);
    Box(volatile const Box& other);

    // Additional parameters OK if they have default values
    Box(Box& other, int i = 42, string label = "Box");
```

定义复制构造函数时，还应定义复制赋值运算符 (=)。 有关详细信息，请参阅[赋值](https://learn.microsoft.com/zh-cn/cpp/cpp/assignment?view=msvc-170)以及[复制构造函数和复制赋值运算符](https://learn.microsoft.com/zh-cn/cpp/cpp/copy-constructors-and-copy-assignment-operators-cpp?view=msvc-170)。

可以通过将复制构造函数定义为已删除来阻止复制对象：

C++复制

```cpp
    Box(const Box& other) = delete;
```

尝试复制对象会产生错误“C2280: 尝试引用已删除的函数”。



## 移动构造函数

移动构造函数是特殊成员函数，它将现有对象数据的所有权移交给新变量，而不复制原始数据。 它采用 rvalue （右值）引用作为其第一个参数，以后的任何参数都必须具有默认值。 移动构造函数在传递大型对象时可以显著提高程序的效率。

C++复制

```cpp
Box(Box&& other);
```

当对象由相同类型的另一个对象初始化时，如果另一对象即将被毁且不再需要其资源，则编译器会选择移动构造函数。 以下示例演示了一种由重载决策选择移动构造函数的情况。 在调用 `get_Box()` 的构造函数中，返回值是 xvalue（过期值）。 它未分配给任何变量，因此即将超出范围。 为了为此示例提供动力，我们为 Box 提供表示其内容的大型字符串向量。 移动构造函数不会复制该向量及其字符串，而是从过期值“box”中“窃取”它，以便该向量现在属于新对象。 只需调用 `std::move` 即可，因为 `vector` 和 `string` 类都实现自己的移动构造函数。

C++复制

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

class Box {
public:
    Box() { 
        std::cout << "default" << std::endl; 
    }
    Box(int width, int height, int length)
       : m_width(width), m_height(height), m_length(length)
    {
        std::cout << "int,int,int" << std::endl;
    }
    Box(Box& other)
       : m_width(other.m_width), m_height(other.m_height), m_length(other.m_length)
    {
        std::cout << "copy" << std::endl;
    }
    Box(Box&& other) : m_width(other.m_width), m_height(other.m_height), m_length(other.m_length)
    {
        m_contents = std::move(other.m_contents);
        std::cout << "move" << std::endl;
    }
    int Volume() { 
        return m_width * m_height * m_length; 
    }
    void Add_Item(string item) { 
        m_contents.push_back(item); 
    }
    void Print_Contents()
    {
        for (const auto& item : m_contents)
        {
            cout << item << " ";
        }
    }
private:
    int m_width{ 0 };
    int m_height{ 0 };
    int m_length{ 0 };
    vector<string> m_contents;
};

Box get_Box()
{
    Box b(5, 10, 18); // "int,int,int"
    b.Add_Item("Toupee");
    b.Add_Item("Megaphone");
    b.Add_Item("Suit");

    return b;
}

int main()
{
    Box b; // "default"
    Box b1(b); // "copy"
    Box b2(get_Box()); // "move"
    cout << "b2 contents: ";
    b2.Print_Contents(); // Prove that we have all the values

    char ch;
    cin >> ch; // keep window open
    return 0;
}
```

如果类未定义移动构造函数，则在没有用户声明的复制构造函数、复制赋值运算符、移动赋值运算符或析构函数时，编译器会生成隐式构造函数。 如果未定义显式或隐式移动构造函数，则原本使用移动构造函数的操作会改用复制构造函数。 如果类声明了移动构造函数或移动赋值运算符，则隐式声明的复制构造函数会定义为已删除。

如果作为类类型的任何成员缺少析构函数或是如果编译器无法确定要用于移动操作的构造函数，则隐式声明的移动构造函数会定义为已删除。

有关如何编写不常用的移动构造函数的详细信息，请参阅[移动构造函数和移动赋值运算符 (C++)](https://learn.microsoft.com/zh-cn/cpp/cpp/move-constructors-and-move-assignment-operators-cpp?view=msvc-170)。



## 显式默认构造函数和已删除构造函数

你可以显式设置默认复制构造函数、设置默认构造函数、移动构造函数、复制赋值运算符、移动赋值运算符和析构函数。 你可以显式删除所有特殊成员函数。

C++复制

```cpp
class Box2
{
public:
    Box2() = delete;
    Box2(const Box2& other) = default;
    Box2& operator=(const Box2& other) = default;
    Box2(Box2&& other) = default;
    Box2& operator=(Box2&& other) = default;
    //...
};
```

有关详细信息，请参阅[显式默认设置的函数和已删除的函数](https://learn.microsoft.com/zh-cn/cpp/cpp/explicitly-defaulted-and-deleted-functions?view=msvc-170)。



## constexpr 构造函数

构造函数在以下情况下可能会声明为 [constexpr](https://learn.microsoft.com/zh-cn/cpp/cpp/constexpr-cpp?view=msvc-170)

- 声明为默认，或是在一般情况下满足 [constexpr 函数](https://learn.microsoft.com/zh-cn/cpp/cpp/constexpr-cpp?view=msvc-170#constexpr_functions) 的所有条件；
- 类没有虚拟基类；
- 每个参数都是[文本类型](https://learn.microsoft.com/zh-cn/cpp/cpp/trivial-standard-layout-and-pod-types?view=msvc-170#literal_types)；
- 主体不是函数 try 块；
- 所有非静态数据成员和基类子对象都会进行初始化；
- 如果类是 (a) 具有变体成员的联合，或是 (b) 具有匿名联合，则只有一个联合成员会进行初始化；
- 类类型的每个非静态数据成员以及所有基类子对象都具有 constexpr 构造函数



## 初始化表达式列表构造函数

如果某个构造函数采用 [`std::initializer_list`](https://learn.microsoft.com/zh-cn/cpp/standard-library/initializer-list-class?view=msvc-170) 作为其参数，并且任何其他参数都具有默认自变量，则当类通过直接初始化来实例化时，会在重载决策中选择该构造函数。 可以使用 initializer_list 初始化可接受它的任何成员。 例如，假设前面演示的 Box 类具有 `std::vector<string>` 成员 `m_contents`。 可以提供如下所示的构造函数：

C++复制

```cpp
    Box(initializer_list<string> list, int w = 0, int h = 0, int l = 0)
        : m_contents(list), m_width(w), m_height(h), m_length(l)
{}
```

随后创建如下所示的 Box 对象：

C++复制

```cpp
    Box b{ "apples", "oranges", "pears" }; // or ...
    Box b2(initializer_list<string> { "bread", "cheese", "wine" }, 2, 4, 6);
```



## 显式构造函数

如果类具有带一个参数的构造函数，或是如果除了一个参数之外的所有参数都具有默认值，则参数类型可以隐式转换为类类型。 例如，如果 `Box` 类具有一个类似于下面这样的构造函数：

C++复制

```cpp
Box(int size): m_width(size), m_length(size), m_height(size){}
```

可以初始化 Box，如下所示：

C++复制

```cpp
Box b = 42;
```

或将一个 int 传递给采用 Box 的函数：

C++复制

```cpp
class ShippingOrder
{
public:
    ShippingOrder(Box b, double postage) : m_box(b), m_postage(postage){}

private:
    Box m_box;
    double m_postage;
}
//elsewhere...
    ShippingOrder so(42, 10.8);
```

这类转换可能在某些情况下很有用，但更常见的是，它们可能会导致代码中发生细微但严重的错误。 作为一般规则，应对构造函数（和用户定义的运算符）使用 **`explicit`** 关键字以防止出现这种隐式类型转换：

C++复制

```cpp
explicit Box(int size): m_width(size), m_length(size), m_height(size){}
```

构造函数是显式函数时，此行会导致编译器错误：`ShippingOrder so(42, 10.8);`。 有关详细信息，请参阅[用户定义的类型转换](https://learn.microsoft.com/zh-cn/cpp/cpp/user-defined-type-conversions-cpp?view=msvc-170)。



## 构造函数顺序

构造函数按此顺序执行工作：

1. 按声明顺序调用基类和成员构造函数。
2. 如果类派生自虚拟基类，则会将对象的虚拟基指针初始化。
3. 如果类具有或继承了虚函数，则会将对象的虚函数指针初始化。 虚函数指针指向类中的虚函数表，确保虚函数正确地调用绑定代码。
4. 它执行自己函数体中的所有代码。

下面的示例显示，在派生类的构造函数中，基类和成员构造函数的调用顺序。 首先，调用基构造函数。 然后，按照在类声明中出现的顺序初始化基类成员。 最后，调用派生构造函数。

C++复制

```cpp
#include <iostream>

using namespace std;

class Contained1 {
public:
    Contained1() { cout << "Contained1 ctor\n"; }
};

class Contained2 {
public:
    Contained2() { cout << "Contained2 ctor\n"; }
};

class Contained3 {
public:
    Contained3() { cout << "Contained3 ctor\n"; }
};

class BaseContainer {
public:
    BaseContainer() { cout << "BaseContainer ctor\n"; }
private:
    Contained1 c1;
    Contained2 c2;
};

class DerivedContainer : public BaseContainer {
public:
    DerivedContainer() : BaseContainer() { cout << "DerivedContainer ctor\n"; }
private:
    Contained3 c3;
};

int main() {
    DerivedContainer dc;
}
```

输出如下：

Output复制

```Output
Contained1 ctor
Contained2 ctor
BaseContainer ctor
Contained3 ctor
DerivedContainer ctor
```

派生类构造函数始终调用基类构造函数，因此，在完成任何额外任务之前，它可以依赖于完全构造的基类。 基类构造函数按派生顺序进行调用 — 例如，如果 `ClassA` 派生自 `ClassB`，而后者派生自 `ClassC`，那么首先调用 `ClassC` 构造函数，然后调用 `ClassB` 构造函数，最后调用 `ClassA` 构造函数。

如果基类没有默认构造函数，则必须在派生类构造函数中提供基类构造函数参数：

C++复制

```cpp
class Box {
public:
    Box(int width, int length, int height){
       m_width = width;
       m_length = length;
       m_height = height;
    }

private:
    int m_width;
    int m_length;
    int m_height;
};

class StorageBox : public Box {
public:
    StorageBox(int width, int length, int height, const string label&) : Box(width, length, height){
        m_label = label;
    }
private:
    string m_label;
};

int main(){

    const string aLabel = "aLabel";
    StorageBox sb(1, 2, 3, aLabel);
}
```

如果构造函数引发异常，析构的顺序与构造的顺序相反：

1. 构造函数主体中的代码将展开。
2. 基类和成员对象将被销毁，顺序与声明顺序相反。
3. 如果是非委托构造函数，所有完全构造的基类对象和成员均会销毁。 但是，对象本身不是完全构造的，因此析构函数不会运行。



## 派生构造函数和扩展聚合初始化

如果基类的构造函数是非公共的，但可由派生类进行访问，那么在 **`/std:c++17`** 模式及更高版本模式下的 Visual Studio 2017 及更高版本中，无法使用空括号来初始化派生类型的对象。

以下示例演示 C++14 一致行为：

C++复制

```cpp
struct Derived;

struct Base {
    friend struct Derived;
private:
    Base() {}
};

struct Derived : Base {};

Derived d1; // OK. No aggregate init involved.
Derived d2 {}; // OK in C++14: Calls Derived::Derived()
               // which can call Base ctor.
```

在 C++17，`Derived` 现被视作聚合类型。 这意味着 `Base` 通过私有默认构造函数进行的初始化将作为扩展的聚合初始化规则的一部分而直接发生。 以前，`Base` 私有构造函数通过 `Derived` 构造函数调用，它之所以能够成功是因为 `friend` 声明。

以下示例展示了在 **`/std:c++17`** 模式下的 Visual Studio 2017 及更高版本中的 C++17 行为：

C++复制

```cpp
struct Derived;

struct Base {
    friend struct Derived;
private:
    Base() {}
};

struct Derived : Base {
    Derived() {} // add user-defined constructor
                 // to call with {} initialization
};

Derived d1; // OK. No aggregate init involved.

Derived d2 {}; // error C2248: 'Base::Base': can't access
               // private member declared in class 'Base'
```



### 具有多重继承的类的构造函数

如果类从多个基类派生，那么将按照派生类声明中列出的顺序调用基类构造函数：

C++复制

```cpp
#include <iostream>
using namespace std;

class BaseClass1 {
public:
    BaseClass1() { cout << "BaseClass1 ctor\n"; }
};
class BaseClass2 {
public:
    BaseClass2() { cout << "BaseClass2 ctor\n"; }
};
class BaseClass3 {
public:
    BaseClass3() { cout << "BaseClass3 ctor\n"; }
};
class DerivedClass : public BaseClass1,
                     public BaseClass2,
                     public BaseClass3
                     {
public:
    DerivedClass() { cout << "DerivedClass ctor\n"; }
};

int main() {
    DerivedClass dc;
}
```

你应看到以下输出：

Output复制

```Output
BaseClass1 ctor
BaseClass2 ctor
BaseClass3 ctor
DerivedClass ctor
```



## 委托构造函数

委托构造函数调用同一类中的其他构造函数，以完成部分初始化工作。 在具有多个全都必须执行类似工作的构造函数时，此功能非常有用。 可以在一个构造函数中编写主逻辑，并从其他构造函数调用它。 在以下简单示例中，Box(int) 将其工作委托给 Box(int,int,int)：

C++复制

```cpp
class Box {
public:
    // Default constructor
    Box() {}

    // Initialize a Box with equal dimensions (i.e. a cube)
    Box(int i) :  Box(i, i, i)  // delegating constructor
    {}

    // Initialize a Box with custom dimensions
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}
    //... rest of class as before
};
```

所有构造函数完成后，完全初始化的构造函数将立即创建对象。 有关详细信息，请参阅[委托构造函数](https://learn.microsoft.com/zh-cn/cpp/cpp/delegating-constructors?view=msvc-170)。



## 继承构造函数 (C++11)

派生类可以使用 **`using`** 声明从直接基类继承构造函数，如下面的示例所示：

C++复制

```cpp
#include <iostream>
using namespace std;

class Base
{
public:
    Base() { cout << "Base()" << endl; }
    Base(const Base& other) { cout << "Base(Base&)" << endl; }
    explicit Base(int i) : num(i) { cout << "Base(int)" << endl; }
    explicit Base(char c) : letter(c) { cout << "Base(char)" << endl; }

private:
    int num;
    char letter;
};

class Derived : Base
{
public:
    // Inherit all constructors from Base
    using Base::Base;

private:
    // Can't initialize newMember from Base constructors.
    int newMember{ 0 };
};

int main()
{
    cout << "Derived d1(5) calls: ";
    Derived d1(5);
    cout << "Derived d1('c') calls: ";
    Derived d2('c');
    cout << "Derived d3 = d2 calls: " ;
    Derived d3 = d2;
    cout << "Derived d4 calls: ";
    Derived d4;
}

/* Output:
Derived d1(5) calls: Base(int)
Derived d1('c') calls: Base(char)
Derived d3 = d2 calls: Base(Base&)
Derived d4 calls: Base()*/
```

Visual Studio 2017 及更高版本：**`/std:c++17`** 模式及更高版本模式下的 **`using`** 语句可将来自基类的所有构造函数引入范围（除了签名与派生类中的构造函数相同的构造函数）。 一般而言，当派生类未声明新数据成员或构造函数时，最好使用继承构造函数。

如果类型指定基类，则类模板可以从类型参数继承所有构造函数：

C++复制

```cpp
template< typename T >
class Derived : T {
    using T::T;   // declare the constructors from T
    // ...
};
```

如果基类的构造函数具有相同签名，则派生类无法从多个基类继承。



## 构造函数和复合类

包含类类型成员的类称为“复合类”。 创建复合类的类类型成员时，调用类自己的构造函数之前，先调用构造函数。 当包含的类没有默认构造函数是，必须使用复合类构造函数中的初始化列表。 在之前的 `StorageBox` 示例中，如果将 `m_label` 成员变量的类型更改为新的 `Label` 类，则必须调用基类构造函数，并且将 `m_label` 变量（位于 `StorageBox` 构造函数中）初始化：

C++复制

```cpp
class Label {
public:
    Label(const string& name, const string& address) { m_name = name; m_address = address; }
    string m_name;
    string m_address;
};

class StorageBox : public Box {
public:
    StorageBox(int width, int length, int height, Label label)
        : Box(width, length, height), m_label(label){}
private:
    Label m_label;
};

int main(){
// passing a named Label
    Label label1{ "some_name", "some_address" };
    StorageBox sb1(1, 2, 3, label1);

    // passing a temporary label
    StorageBox sb2(3, 4, 5, Label{ "another name", "another address" });

    // passing a temporary label as an initializer list
    StorageBox sb3(1, 2, 3, {"myname", "myaddress"});
}
```