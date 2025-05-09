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
<!--by 宁国康-->
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

以下是 **`tut03_findhists`** 的核心代码示例：

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
<!--by 宁国康-->
```

---
