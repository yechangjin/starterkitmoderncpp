# 词汇表

智能指针（Smart Pointer） ：shared_ptr（共享指针）
引用计数（Reference Counting） ：use_count（使用计数）
构造函数（Constructor） ：constructor
析构函数（Destructor） ：destructor
拷贝构造函数（Copy Constructor） ：copy constructor
赋值运算符（Assignment Operator） ：assignment operator
<!--叶昌金-->




代码2中的术语词汇有:
1.File handling with smart pointers
文件处理与智能指针
2.Smart pointers
智能指针
3.Lifetime of a file
文件的生命周期
4.Out-of-scope
超出作用域
5.Destructor
析构函数
6.TFile destructor
7.TFile 析构函数
8.Block statement
块语句
9.std unique_ptr
标准库中的唯一指针
10.TFile::Open
打开文件的方法
11.gFile
全局文件指针
12.SetDirectory(nullptr)
将对象与目录断开连接
13.TCanvas
画布对象
14.Draw()
绘图方法
<!--程世超-->




## ROOT 相关词汇

| 词汇                  | 描述                             |
|-----------------------|----------------------------------|
| TFile                 | ROOT 文件操作类                  |
| TH1                   | ROOT 一维直方图基类              |
| TObjArray             | ROOT 对象数组容器                |
| TKey                  | ROOT 文件对象键                  |
| gDirectory            | ROOT 全局目录指针               |
| SetDirectory          | 直方图目录关联方法               |
| READ Mode             | 只读模式                         |
| Zombie File           | 僵尸文件状态                     |
| File I/O              | 文件输入输出                     |
| Histogram Tag         | 直方图标签                       |
| Client-Server Model   | ROOT 文件目录系统的客户端 - 服务器模型 |
| Template              | 模板编程（TObjArray 等容器实现基础） |

## C++ 相关词汇

| 词汇                  | 描述                             |
|-----------------------|----------------------------------|
| std::unique_ptr       | 独占所有权智能指针（管理任意对象生命周期） |
| static_cast           | 静态类型转换                     |
| auto                  | 自动类型推导                     |
| nullptr               | 空指针常量                       |
| const char*           | 常量字符指针                     |
| virtual               | 虚函数关键字                     |

## 编程概念词汇

| 词汇                  | 描述                             |
|-----------------------|----------------------------------|
| Smart Pointers        | 智能指针                         |
| Memory Management     | 内存管理                         |
| Static Type Casting   | （原 Dynamic Casting）静态类型转换 |
| Error Handling        | 错误处理                         |
| Iterator              | 迭代器                           |
<!--宁国康-->



| 中文术语 | 英文术语                     |                说明                           |
| 关联容器 | Associative Container       | 用于存储键值对的数据结构，如`std::map`和`TMap`。 |
| 绘图样式 | Plot Style                  | 定义图形的颜色、标记等属性的集       合。        |
| 数据分布 | Data Distribution           | 数据的分布情况，例如不同周期的数据。             |
| 周期比较 | Period-by-Period Comparison | 对不同周期的数据进行比较分析。                   |
| 直方图   | Histogram                   | 一种图形，用于展示数据的分布情况。               |
<!--陆德劲-->




    工厂函数 (Factory Function)
工厂函数是一种设计模式，用于动态创建对象而无需暴露具体实现细节。它通过一个统一的接口（通常是函数或方法）返回不同类型的对象实例，常用于：
封装对象创建逻辑
实现多态性
减少代码重复
在示例代码中，createStyle 就是一个典型的工厂函数。如下所示：
auto createStyle = [](int color, int marker) {
    return [=](auto& obj) {  // 返回一个泛型Lambda（闭包）
        obj.SetMarkerColor(color);
        obj.SetLineColor(color);
        obj.SetMarkerStyle(marker);
        if constexpr (std::is_base_of_v<TH1, std::decay_t<decltype(obj)>>) {
            obj.SetFillColorAlpha(color, 0.3);  // 仅对TH1派生类生效
        }
    };
};

    样式闭包(Style Closure)
样式闭包 是结合函数式编程的闭包特性和面向对象样式配置 的一种技术，用于封装图形对象的样式逻辑。

通用Lambda (Generic Lambda)
C++14特性，允许Lambda参数使用auto实现泛型

    类型自适应（Type Adaptation）
类型自适应 指代码能够根据操作对象的类型自动调整行为，在ROOT绘图模板项目中体现为：
编译时类型识别：通过C++模板和if constexpr区分TH1（直方图）和TGraph（曲线图）
差异化处理：对直方图添加填充色，曲线图仅设置线条和标记样式
零运行时开销：类型检查在编译期完成，不影响执行效率

    类型萃取（Type Traits）
类型萃取是C++模板元编程的核心技术，通过编译期类型检查实现：
类型特性查询（如是否是指针、基类判断）
类型转换（如移除引用/const修饰）
条件编译控制
[术语关系](术语关系.png)
<!--梁宏军-->






| 中文术语                       | 英文翻译/解释                          |
|--------------------------------|---------------------------------------|
| 喷注                           | Jet                                   |
| 横向动量 (pT)                  | Transverse Momentum (pT)             |
| 赝快度 (eta)                   | Pseudorapidity (eta)                 |
| 方位角 (phi)                   | Azimuthal Angle (phi)                |
| 组成粒子数                     | Number of Constituents               |
| 数据结构                       | Data Structure                       |
| STL容器                        | STL Container                        |
| Lambda函数                     | Lambda Function                      |
| 降序排序                       | Descending Order Sorting             |
| 时间复杂度                     | Time Complexity                      |
| `<algorithm>`头文件            | `<algorithm>` Header                 |
| 稳定排序                       | Stable Sort                          |
| 空容器检查                     | Empty Container Check               |
| 遍历                           | Iterate                              |
| 编译失败                       | Compilation Failure                 |
| 断言                           | Assert                               |
| 边界情况                       | Edge Cases                           |
| 异常处理                       | Exception Handling                  |
| 预分配内存                     | Preallocate Memory                  |
| 性能优化                       | Performance Optimization            |
| ROOT框架                       | ROOT Framework                      |
| ATLAS/CMS实验                  | ATLAS/CMS Experiments               |
| 横向动量降序                   | Descending Order of Transverse Momentum |
| 组成粒子数阈值                 | Constituent Number Threshold        |
| 物理属性                       | Physical Properties                 |
| 数据结构设计                   | Data Structure Design               |
| 降序排列                       | Descending Order Arrangement        |
| 稳定排序                       | Stable Sorting                      |
| 时间复杂度O(N log N)           | Time Complexity O(N log N)          |
| 空值检查                       | Null Check                          |
| 排序逻辑                       | Sorting Logic                       |
| 调试技巧                       | Debugging Techniques                |
| 扩展练习                       | Extended Exercises                  |
| 核心知识点                     | Core Concepts                       |
| 高能物理实验                   | High-Energy Physics Experiments     |
<!--李诚东-->








# 专业英语术语与中文对照表

---

## **编程与框架相关**
1. **C++ 编译器** - C++ Compiler  
2. **ROOT 框架** - ROOT Framework  
3. **Jupyter Notebook** - Jupyter Notebook  
4. **Python** - Python  
5. **依赖项** - Dependencies  
6. **环境变量** - Environment Variables  
7. **内核** - Kernel  
8. **调试** - Debugging  
9. **GitHub issues** - GitHub Issues  

---

## **C++ 语言特性**
1. **C++11** - C++11  
2. **对象初始化** - Object Initialization  
3. **构造函数** - Constructor  
4. **默认构造函数** - Default Constructor  
5. **成员变量** - Member Variables  
6. **隐式类型转换** - Implicit Type Conversion  
7. **窄化转换** - Narrowing Conversion  
8. **初始化列表** - Initializer List (`std::initializer_list`)  
9. **模板类** - Template Class  
10. **模板函数** - Template Function  
11. **结构体** - Struct  
12. **POD 对象** - POD (Plain Old Data) Objects  
13. **双精度浮点数** - Double-Precision Floating-Point  
14. **编译失败** - Compilation Failure  

---

## **标准库组件**
1. **std::vector** - `std::vector` (动态数组容器)  
2. **emplace_back** - `emplace_back` (容器方法)  
3. **decltype** - `decltype` (类型推导关键字)  

---

## **代码示例相关**
1. **颜色值** - Color Value (如 `kRed`, `kBlue`)  
2. **标记样式** - Marker Style (如 `24`, `25`)  
3. **顶点坐标** - Vertex Coordinate (`fvz`)  
4. **轨道数量** - Track Count (`fNtrack`)  

---

## **工具与命令**
1. **终端** - Terminal  
2. **克隆项目** - Clone a Repository (`git clone`)  
3. **conda install** - Conda Install (包管理命令)  
4. **pip install** - Pip Install (Python 包安装命令)  

---

## **规则与总结**
1. **优先使用 {} 初始化** - Prefer `{}` Initialization  
2. **类型兼容性** - Type Compatibility  

---
<!--苏国权-->





‌核心框架术语‌
‌RDataFrame‌
ROOT提供的高性能数据分析框架，采用声明式编程范式，支持多线程和惰性求值

‌Define()‌
数据列定义方法，用于创建新的计算分支（如：pt = sqrt(px²+py²)）

‌Filter()‌
数据筛选方法，通过布尔表达式选择满足条件的记录

‌Histo1D()/Histo2D()‌
直方图生成方法，自动进行数据分箱统计（1D/2D版本）

‌Snapshot()‌
数据持久化方法，将处理后的数据集保存为ROOT文件

‌ImplicitMT‌
隐式多线程支持，通过ROOT::EnableImplicitMT()启用并行计算

‌物理量术语‌
‌Transverse Momentum (pT)‌

‌Pseudorapidity (η)‌

‌Azimuthal Angle (φ)‌

‌TPC Clusters‌
时间投影室（Time Projection Chamber）簇射数，反映轨迹重建质量

‌ITS Clusters‌
内径迹系统（Inner Tracking System）簇射数，表征顶点重建精度

‌可视化术语‌
‌TCanvas‌
ROOT的画布对象，用于管理多个图形输出

‌COLZ‌
二维直方图的颜色渐变绘制选项

‌Error Bars‌
统计误差条，反映直方图分箱的统计不确定性

‌数据分析术语‌
‌Lazy Evaluation‌
惰性求值机制，延迟执行直到需要结果时才进行计算

‌Action‌
终结操作（如Fill直方图、保存文件），触发实际计算

‌Report()‌
生成过滤器性能报告，显示各阶段的通过率

‌Range()‌
数据切片方法，用于处理部分数据样本

‌存储相关‌
‌TTree‌
ROOT的列式存储结构，支持高效大数据处理

‌CERNBox‌
欧洲核子研究中心（CERN）的云存储服务，常用于保存物理数据

‌ROOT File‌
专用二进制文件格式，支持复杂数据结构的持久化存储

‌统计指标‌
‌Mean/StdDev‌
均值/标准差计算，反映数据分布的中心趋势和离散程度

‌Aggregate()‌
自定义聚合操作，实现复杂统计量的批处理计算

‌Event Loop‌
隐式事件循环，RDataFrame在后台自动管理数据遍历

‌代码结构‌
‌Lambda Expression‌
C++11的匿名函数，用于定义自定义操作（如：[&](auto x){...}）

‌Tuple‌
元组结构，用于打包多个计算结果（如：std::make_tuple(...)）
<!--许志繁-->

