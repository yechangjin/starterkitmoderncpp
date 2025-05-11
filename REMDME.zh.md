```markdown
# ROOT C++ 数据分析教程集

一套基于ROOT框架和现代C++特性的数据分析教程，涵盖智能指针、泛型编程、数据可视化等核心技能。

![ROOT Framework Logo](https://root.cern/img/logos/ROOT_Logo/ROOT_Logo_vertical.png)

## 📋 项目概述
- **核心框架**: ROOT (CERN开发的高能物理数据分析工具)
- **语言特性**: C++11/14 (智能指针、Lambda表达式、初始化列表等)
- **教程主题**: 8个独立模块，覆盖文件操作/数据排序/样式管理等场景
- **数据格式**: 使用`.root`二进制文件存储直方图/树状数据

## 🛠️ 环境配置
###项目克隆
运行以下命令克隆项目:
```bash
git clone https://github.com/yechangjin/starterkitmoderncpp.git 
cd starterkitmoderncpp
```
### 依赖安装
1. **ROOT框架** ([官网安装指南](https://root.cern/install/))
   ```bash
   # Linux示例（Ubuntu）
   wget https://root.cern/download/root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz
   tar -xzvf root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz
   source root/bin/thisroot.sh
   ```
2. **编译器**: GCC 9+ 或 Clang 10+ (需支持C++14)
3. **编辑器**: VS Code + 扩展包
   - [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
   - [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)

### 数据准备
```text
项目根目录/
├── data/                  # 存放所有.root数据文件
│   ├── hsimple.root       # 示例直方图数据
│   └── rdfexample.root    # RDataFrame测试数据
│                          # 教程代码文件
├── tut01_sharedptr.ipynb           #共享指针引用计数
├── tut02_filereader.ipynb          #智能指针管理ROOT文件
├── tut03_findhists.ipynb           #动态查找直方图
├── tut04_periodcomparison.ipynb    #多实验周期数据对比
├── tut05_genericstyles.ipynb       #泛型图形样式
├── tut06_leadingjet.ipynb          #喷注数据排序
├── tut07_initialization.ipynb      #C++11初始化方法对比
└── tut08_rdataframe.ipynb          #高效数据分析框架
              
```
## 🚀 快速开始

### Jupyter模式（推荐）
```bash
# 启动ROOT内核的Jupyter
root --notebook
```
- 在浏览器中打开`.ipynb`文件
- 选择内核为 **ROOT C++**
- 逐个执行Cell观察输出

![Jupyter界面示例](https://jupyter.org/assets/homepage-main.png)

### 命令行编译
```bash
# 示例：编译tut03文件
g++ tutorials/tut03_findhists.C -o output \
    $(root-config --cflags --libs --glibs)
./output
```

## 📚 教程目录

| 文件                      | 核心内容                     | 关键技术点                  |
|--------------------------|----------------------------|---------------------------|
| `tut01_sharedptr`        | 共享指针引用计数             | `std::shared_ptr`         |
| `tut02_filereader`       | 智能指针管理ROOT文件         | `std::unique_ptr<TFile>`  |
| `tut03_findhists`        | 动态查找直方图               | `TObjArray`遍历           |
| `tut04_periodcomparison` | 多实验周期数据对比           | `std::map`样式管理        |
| `tut05_genericstyles`    | 泛型图形样式                 | C++14 Lambda闭包          |
| `tut06_leadingjet`       | 喷注数据排序                 | `std::sort`+自定义比较器  |
| `tut07_initialization`   | C++11初始化方法对比          | 列表初始化`{}`            |
| `tut08_rdataframe`       | 高效数据分析框架             | ROOT::RDataFrame          |
<!--宁国康-->



**`tut01_sharedptr`**



共享指针的使用
Payload 类
作用 ：作为共享资源的示例类，用于可视化构造函数和析构函数的调用。
成员函数 ：
构造函数（Constructor） ：当创建 Payload 对象时调用，输出提示信息。
拷贝构造函数（Copy Constructor） ：当以已存在的 Payload 对象初始化新对象时调用，输出提示信息。
赋值运算符（Assignment Operator） ：当将一个 Payload 对象赋值给另一个已存在的对象时调用，输出提示信息。
析构函数（Destructor） ：当 Payload 对象生命周期结束时调用，输出提示信息。
Client 类
成员变量 ：std::shared_ptr<Payload> fPayload，用于管理对 Payload 对象的共享指针。
构造函数 ：
无参构造函数（Constructor） ：创建 Client 对象时调用，初始化共享指针，并输出提示信息。
带参构造函数（Constructor with parameter） ：接受一个 Payload 指针，将其封装到共享指针中，输出当前访问该 Payload 的客户端数量。
拷贝构造函数（Copy Constructor） ：当以已存在的 Client 对象初始化新对象时调用，共享同一个 Payload 对象，并输出当前访问该 Payload 的客户端数量。
赋值运算符（Assignment Operator） ：当将一个 Client 对象赋值给另一个已存在的对象时调用，更新共享指针指向，并输出当前访问该 Payload 的客户端数量。
析构函数（Destructor） ：当 Client 对象生命周期结束时调用，输出当前访问该 Payload 的客户端数量（删除前）。
主函数（main）
创建一个基础客户端对象 base，并为其分配新的 Payload 资源，此时引用计数为 1。
创建一个包含 10 个其他客户端对象的向量 others，每个对象都是通过拷贝 base 创建的，它们共享同一个 Payload 资源，引用计数逐渐增加。
删除向量中的所有其他客户端对象，每次删除后引用计数减少，但 Payload 资源仍存在，因为还有 base 指向它。
最后删除基础客户端对象 base，此时引用计数变为 0，自动删除 Payload 对象，释放资源。
<!--叶昌金-->






**`tut02_filereader`**
文件处理与智能指针
作者 contribution: 用中文解释了智能指针在文件处理中的重要性，强调了智能指针如何帮助管理文件对象的生命周期，防止对象意外地保持文件连接。
文件读取示例
作者 : 用中文提供了代码示例，详细说明了如何使用智能指针读取文件并获取对象，同时解释了如何在作用域结束时自动关闭文件，以及如何将读取的对象与文件断开连接。
绘图示例
作者 : 用中文提供了代码示例，展示了如何在内存中绘制从文件读取的直方图对象。
检查文件是否仍打开
作者 : 用中文提供了一个简单的代码片段，用于检查文件是否仍然打开，并解释了如何通过检查 gFile 指针来判断文件状态。

<!--程世超-->






**`tut03_findhists`** ：

---

### 动态查找直方图（tut03）
```cpp
TH1* findHist(const char* filename, const char* histtag) {
    TH1* hist = nullptr;
    std::unique_ptr<TFile> reader(TFile::Open(filename, "READ")); // 智能指针管理文件
    
    // 检查文件是否有效
    if (!reader || reader->IsZombie()) {
        std::cerr << "Error: Failed to open file " << filename << std::endl;
        return nullptr;
    }

    // 获取第一个键对应的对象（假设是TObjArray）
    TKey* firstKey = static_cast<TKey*>(reader->GetListOfKeys()->At(0));
    TObjArray* histList = dynamic_cast<TObjArray*>(firstKey->ReadObj());
    
    // 遍历对象数组查找匹配的直方图
    if (histList) {
        for (auto obj : *histList) {
            TH1* candidate = dynamic_cast<TH1*>(obj); // 动态类型转换
            if (candidate && std::string(candidate->GetName()).find(histtag) != std::string::npos) {
                hist = candidate;
                hist->SetDirectory(nullptr); // 断开与文件的关联
                break;
            }
        }
    }
    return hist;
}
```

---

### 代码解析
| 关键步骤                  | 技术点                                                                |
|---------------------------|----------------------------------------------------------------------|
| **智能指针管理文件**      | `std::unique_ptr<TFile>` 确保文件自动关闭，避免资源泄漏                 |
| **动态类型转换**          | `dynamic_cast<TObjArray*>` 和 `dynamic_cast<TH1*>` 确保类型安全        |
| **直方图名称匹配**        | `std::string::find` 实现模糊匹配（如查找名称包含 `"hPt"` 的直方图）      |
| **断开文件关联**          | `SetDirectory(nullptr)` 防止直方图随文件关闭被销毁                      |

---

### 使用示例
```cpp
// 查找并绘制直方图
TH1* hPt = findHist("./data/10111000101_trackqa.root", "hPt");
if (hPt) {
    TCanvas* canvas = new TCanvas("canvas", "Plot", 800, 600);
    hPt->Draw();
    canvas->SaveAs("hPt_plot.png"); // 导出为图片
} else {
    std::cerr << "Error: Histogram hPt not found!" << std::endl;
}
```

---

### 预期输出
1. 成功找到直方图时：  
   ![hPt_plot.png](https://via.placeholder.com/800x600/EEE/000?text=hPt+Histogram)
2. 失败时终端输出：  
   ```text
   Error: Histogram hPt not found!
   ```

---

## ⚠️ 常见问题

### 编译错误：未找到ROOT头文件
```bash
# 确认环境变量
echo $ROOTSYS
# 重新配置编译命令
$(root-config --cflags --libs)
```

### 运行时数据文件缺失
```text
错误信息：Error in <TFile::TFile>: file ./data/xxx.root does not exist
解决方法：
1. 检查data/目录结构
2. 从项目仓库下载示例数据
```
<!-- 宁国康-->



**`tut04_periodcomparison`**
1. 定义绘图样式
*PlotStyle类封装数据集的视觉样式属性：
/**
 * @class PlotStyle
 * @brief 数据集可视化样式封装类
 */
class PlotStyle : public TObject {
    Color_t fColor;  // 颜色编码
    Style_t fMarker; // 标记样式
    
public:
    PlotStyle() : TObject(), fColor(0), fMarker(0) {}
    PlotStyle(Color_t color, Style_t marker) 
        : TObject(), fColor(color), fMarker(marker) {}
    
    // 样式设置方法
    void SetColor(Color_t color) { fColor = color; }
    void SetMarker(Style_t marker) { fMarker = marker; }
    
    // 样式获取方法
    Color_t GetColor() const { return fColor; }
    Style_t GetMarker() const { return fMarker; }
    
    /**
     * @brief 将样式应用于ROOT图形对象
     * @tparam T 图形对象类型(TH1, TGraph等)
     * @param obj 图形对象指针
     */
    template<typename T>
    void SetStyle(T* obj) const {
        obj->SetMarkerColor(fColor);
        obj->SetLineColor(fColor);
        obj->SetMarkerStyle(fMarker);
    }
};
#2. 配置周期样式
使用关联容器管理各周期样式：
// 使用std::map配置
std::map<std::string, PlotStyle> styleMap = {
    {"LHC17h", {kRed, 24}},    // 红色，上三角标记
    {"LHC17i", {kBlue, 25}},   // 蓝色，下三角标记
    {"LHC17k", {kGreen, 26}},  // 绿色，方形标记
    {"LHC17m", {kViolet, 27}}  // 紫色，圆形标记
};

// 使用ROOT的TMap配置
TMap styleTMap;
styleTMap.Add(new TObjString("LHC17h"), new PlotStyle(kRed, 24));
styleTMap.Add(new TObjString("LHC17i"), new PlotStyle(kBlue, 25));
styleTMap.Add(new TObjString("LHC17k"), new PlotStyle(kGreen, 26));
styleTMap.Add(new TObjString("LHC17m"), new PlotStyle(kViolet, 27));

<!--陆德劲-->





**`tut05_genericstyles`**
cpp
// 创建样式闭包
auto createStyle = [](int color, int marker) {
    return [=](auto& obj) {
        obj.SetMarkerColor(color);
        obj.SetLineColor(color);
        obj.SetMarkerStyle(marker);
        obj.SetMarkerSize(1.5);
        if constexpr (std::is_base_of_v<TH1, std::decay_t<decltype(obj)>>) {
            obj.SetFillColorAlpha(color, 0.3);
        }
    };
};

// 创建具体样式
auto graphStyle = createStyle(kRed, 24);
auto histoStyle = createStyle(kBlue, 25);

// 应用样式
graphStyle(*testgraph);
histoStyle(*testhisto);

// 绘制图形
TCanvas *c1 = new TCanvas("c1", "Plot Demo", 800, 600);
testhisto->Draw("hist");
testgraph->Draw("P same");
c1->Update();
或在Jupyter Notebook中直接运行.ipynb文件
在Jupyter Notebook切换到root c++内核
[切换内核](切换内核.png)
然后运行代码

<!--梁宏军-->





**`tut06_leadingjet`**
## 1.事件中主要喷注（Leading Jets）分析教程
**目标**：在包含多个喷注的粒子对撞事件中，识别并提取横向动量（pT）最高的两个喷注。本指南描述完整的数据处理流程，适用于ROOT框架下的物理数据分析。
**适用场景**：粒子物理数据分析、ROOT框架初学者、喷注属性处理。
---

## 2.代码结构解析📦
### 2.1 喷注数据结构定义及数据结构设计
- **`struct jet`**：存储喷注的物理属性：
  - `fPt`（横向动量）、`fEta`（赝快度）、`fPhi`（方位角）、`fNConstituents`（组成粒子数）。
---

### 2.2 喷注数据初始化
- **`std::vector<jet> jets`**：示例数据包含5个喷注，数值仅为演示用途。
- 示例数据按 `{pT, eta, phi, n}` 格式初始化。
---
<!--李诚东-->





**`tut07_initialization`**
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
<!--苏国权-->






**`tut08_rdataframe`**
// ========== 1. 数据加载与验证 ==========
{
    // 验证输入文件完整性
    std::unique_ptr<TFile> branchreader(TFile::Open("./data/rdfexample.root", "READ"));
    auto testtree = static_cast<TTree*>(branchreader->Get("tracktree"));
    testtree->Print();  // 打印树结构信息
}

// 创建RDataFrame实例
ROOT::RDataFrame trackframe("tracktree", "./data/rdfexample.root");

// ========== 2. 定义新物理量分支 ==========
auto df_with_branches = trackframe
    // 横向动量: pt = √(px² + py²)
    .Define("pt",  "sqrt(px*px + py*py)")  
    
    // 赝快度: η = -ln[tan(θ/2)], θ = arctan(√(px²+py²)/pz)
    .Define("eta", "-log(tan(0.5*atan2(sqrt(px*px + py*py), pz)))")  
    
    // 方位角: φ = atan2(py, px)
    .Define("phi", "atan2(py, px)");       

// 打印更新后的列清单
std::cout << "\n更新后的列清单:" << std::endl;
for (const auto &col : df_with_branches.GetColumnNames()) {
    std::cout << " • " << col << std::endl;
}
<!--许志繁-->
```
---





