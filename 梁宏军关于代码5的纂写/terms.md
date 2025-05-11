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