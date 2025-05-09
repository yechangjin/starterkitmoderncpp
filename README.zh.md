# tut07_initialization

# 如何运行这个项目
以下是运行 `starterkitmoderncpp` 项目的详细指南。
---
## 一、环境要求
1. **C++ 编译器**
   - 需要支持 C++11 或更高版本的编译器，例如 `g++` 或 `clang++`。
2. **ROOT 框架**
   - 该项目的 Jupyter Notebook 使用了 ROOT C++ 内核。
   - 请安装 ROOT 框架以支持运行。
3. **Jupyter Notebook**
   - 必须安装 Jupyter Notebook 以运行 `.ipynb` 文件。
4. **Python 和相关依赖**
   - 确保安装了 Python 和 Jupyter Notebook 所需的依赖包。
---

## 二、安装步骤
### 1. 克隆该项目到本地
运行以下命令克隆项目：
```bash
git clone https://github.com/qqx123456/starterkitmoderncpp.git
cd starterkitmoderncpp

```2.安装root框架
如果您没有安装ROOT框架，请参考[官方文档](https://root.cern/install/)进行安装。
确保 root 命令可以在终端运行。

    3. 安装 Jupyter Notebook
如果您没有安装 Jupyter Notebook，请参考[官方文档](https://jupyter.org/install)进行安装。
可以使用以下命令安装：
```bash
pip install notebook

    4. 安装 ROOT C++ 内核
如果您没有安装 ROOT C++ 内核，请参考[官方文档](https://root.cern/install/all-in-one/)进行安装。
运行以下命令，为Jupyter Notebook添加ROOT C++ 内核：
conda install -c conda-forge root
   ```


### 运行项目
1. 启动 Jupyter Notebook：
   ```bash
   jupyter notebook
   ```
2. 打开项目文件
在浏览器中打开Jupyter Notebook:
找到并打开项目中的 .ipynb 文件，点击打开。例如：`tut07_initialization.ipynb`。

3. 选择内核
   在 Jupyter Notebook 的顶部菜单中，选择Kernel，然后选择 `Change Kernel`，选择 `Root C++` 内核。

4. 按顺序运行代码单元格
   在Notebook中，按顺序运行每个代码单元格，观察运行结果。



## 三、注意事项
1. 检查依赖项
   - 确保安装了所有依赖项，如 ROOT 框架、Jupyter Notebook、Python 等。
   - 确保运行环境中有 ROOT 环境变量。
2. 内核兼容性
   - 吐过在 Jupyter Notebook 中无法运行代码，请检查是否选择了正确的内核，如 `Root C++` 内核。

3. 调试问题
   - 如果遇到问题，建议查看相关的错误信息并根据提示进行修复，或者在 [GitHub issues](https://github.com/qqx123456/starterkitmoderncpp/issues) 中提出。




# 对'tut07_initialization.ipynb'的说明
   ```
`tut07_initialization.ipynb` 演示了现代 C++11 中对象初始化的不同方法和技术，包括使用 `()` 和 `{}` 进行初始化的区别、POD（Plain Old Data）对象的初始化方式、以及通过初始化列表（initializer list）动态存储数据的用法。此教程适合希望深入了解 C++ 初始化机制的开发者。
```


# 一、C++11中的对象初始化

## () 和 {} 之间的区别

对象可以使用普通的()和花括号{}进行初始化。花括号有一些限制，这可能有助于使代码更加健壮。

## 核心区别
- **圆括号 `()`**  
  允许隐式类型转换，但可能导致窄化转换（如 `double` → `int` 导致精度丢失）。
- **大括号 `{}`**  
  严格禁止窄化转换，增强代码健壮性。

## 示例代码
```cpp
class SimpleEvent {
    int fhtrack;     //跟踪代码
    double fvz;      //顶点坐标

public:
    SimpleEvent(int ntrack, double vz) : fhtrack(ntrack), fvz(vz) {}
};

// 合法初始化
SimpleEvent nb1(10, 0.5);    // 圆括号，隐式 int → double
SimpleEvent cb1{10, 0.5};    // 大括号，合法

// 非法初始化（窄化转换）
// SimpleEvent nb2(5.0, 10);  // 警告：double → int 精度丢失（编译器可能通过）
// SimpleEvent cb2{5.0, 10};  // 错误：大括号禁止窄化（编译失败）

规则总结
1.优先使用 {} 初始化以避免意外窄化。
2.若需隐式转换，使用 （） 进行初始化，但需明确类型兼容性。
```


##原文档解析：
C++ 类 `SimpleEvent` 代码解析

## 类定义

`SimpleEvent` 类包含以下成员变量：
- `int fNtrack`: 用于存储整数类型的轨道数量。
- `double fVz`: 用于存储双精度浮点数类型的垂直位置。

## 构造函数

  * **默认构造函数**
  ```cpp
SimpleEvent() : fNtrack(0), fVz(DBL_MIN) {}

## 类定义
```cpp
class SimpleEvent {
    int fhttrack;
    double fvz;

public:
    // 构造函数与方法定义
};
    SimpleEvent() : fhttrack(0), fvz(OBL_PUN) {} //初始化 fNtrack 为 0，fVz 为 DBL_MIN（双精度浮点数的最小值）。这种初始化方式在对象未提供明确初始化参数时使用。
    SimpleEvent(int ntrack, double vz) : fhttrack(ntrack), fvz(vz) {} //根据传入的参数 ntrack 和 vz 初始化 fNtrack 和 fVz。使对象能够在创建时就被赋予特定的值。

    void SetNTrack(int ntrack) { fNtrack = ntrack; } //用于设置 fNtrack 的值，参数 ntrack 是要设置的新值。
    void SetVz(double vz) { fVz = vz; } //用于设置 fVz 的值，参数 vz 是要设置的新值。

    int GetNTrack() const { return fNtrack; } //用于获取 fNtrack 的值，返回类型为 int，并且该函数被声明为 const，表示调用此函数不会修改对象的状态。
    double GetVz() const { return fVz; } //用于获取 fVz 的值，返回类型为 double，同样被声明为 const。

    
使用 () 初始化对象。使用圆括号初始化对象。其中有一个案例使用了缩小的隐式类型转换（值的精度低于构造函数中的参数精度）。
SimpleEvent nb1(10, 0.5);
SimpleEvent nb2(5., 10.);    // 缩小。由于从 double 类型转换为 int 类型导致精度损失。
// 那么相反方向呢：隐式转换中精度“增加”。
SimpleEvent nb3{15, 20};

使用 {} 初始化对象。其中有一个案例使用了缩小的隐式类型转换（值的精度低于构造函数中的参数精度）。在缩小的情况下会发生什么？
SimpleEvent cb1{10, 0.5};
SimpleEvent cb2{5., 10.};    // 缩小。由于从 double 类型转换为 int 类型导致精度损失。
// “那么相反方向呢：隐式转换中精度“提升”。
SimpleEvent cb3{15, 20};
```


# 二、POD对象
```cpp
普通旧数据结构（POD）仅由一组属性组成——不适用面向对象原则（如封装、继承等）。在C++中，POD对象由结构体表示。如果对象所持有的数据量有限且功能有限，POD对象可能会很有用。


struct Style {  // 定义一个名为 Style 的结构体，用于存储样式信息
    Color_t fColor;   // 成员变量 fColor，类型为 Color_t，用于存储颜色信息
    Style_t fMarker;  // 成员变量 fMarker，类型为 Style_t，用于存储标记样式信息

    template<typename t>    // 定义模板函数 SetStyle，接受一个任意类型 t 的指针参数 object
    void SetStyle(t * object) {
        object->SetMarkerColor(fColor);   // 调用对象的 SetMarkerColor 方法，设置标记颜色为 fColor
        object->SetMarkerStyle(fMarker);    // 调用对象的 SetMarkerStyle 方法，设置标记样式为 fMarker
        object->SetLineColor(fColor);   // 调用对象的 SetLineColor 方法，设置线条颜色为 fColor
    }
};

这个结构体看起来熟悉吗？
使用圆括号 () 构造 POD 对象
Style s1(kRed, 24);    // Style s1(kRed, 24); 中，kRed 是颜色值，24 是标记样式值
// 这种方式可能允许隐式类型转换

使用花括号 {} 初始化 POD 对象
Style s2{kBlue, 25};    // Style s2{kBlue, 25}; 中，kBlue 是颜色值，25 是标记样式值
// 这种方式更严格，可能防止隐式类型转换导致的精度损失

```


# 三、初始化列表

在上面的例子中，构造函数使用花括号被调用，并且正好有两个参数。如果需要存储的参数数量应该是动态的，那该怎么办？

```cpp
template<typename t>
class container {  // 定义一个模板类 container，用于存储任意类型的数据
    std::vector<t> fData;  // 成员变量 fData，类型为 std::vector，用于存储数据
public:
    container() : fData() {}  // 默认构造函数，初始化 fData 为空

    // 使用初始化列表的构造函数，接受一个 std::initializer_list 作为参数
    container(std::initializer_list<t> in) : fData() {
        for(decltype(in.begin()) it = in.begin(); it != in.end(); it++) { 
            fData.emplace_back(*it);  // 将传入的初始化列表中的元素添加到 fData 中
        }
    }

    // 获取 fData 的引用
    const std::vector<t> &Data() const { return fData; }
};


创建一个包含几个值的容器并打印它。

```cpp
container<int> test1{1,2,3,4,5}; // 使用大括号初始化 container 对象 test1，包含整数 1 到 5
container<int> test2 = {6,7,8,9,10}; // 使用等号和大括号初始化 container 对象 test2，包含整数 6 到 10
std::cout << "Container1: " << test1 << "; Container2: " << test2 << std::endl; // 打印 test1 和 test2 的内容

初始化列表总是包含相同类型的对象！
```



























































