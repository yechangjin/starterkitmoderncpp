<!--by 李诚东-->

```markdown
# ROOT C++ 数据分析教程集

一套基于ROOT框架和现代C++特性的数据分析教程，涵盖智能指针、泛型编程、数据可视化等核心技能。

![ROOT Framework Logo](https://root.cern/img/logos/ROOT_Logo/ROOT_Logo_vertical.png)

## 📋 项目概述
- **核心框架**: ROOT (CERN开发的高能物理数据分析工具)
- **语言特性**: C++11/14 (智能指针、Lambda表达式、初始化列表等)
- **教程主题**: 8个独立模块，覆盖文件操作/数据排序/样式管理等场景
- **数据格式**: 使用`.root`二进制文件存储直方图/树状数据

## 🛠 环境配置

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
├── data/                  # 存放所有.root数据文件
│   ├── hsimple.root       # 示例直方图数据
│   └── rdfexample.root    # RDataFrame测试数据
│                          # 教程代码文件
├── tut01_sharedptr.ipynb           #共享指针引用计数
├── tut02_filereader.ipynb          #智能指针管理ROOT文件
├── tut03_findhists.ipynb           #动态查找直方图
├── tut04_periodcomparison.ipynb    #多实验周期数据对比
├── tut05_genericstyles.ipynb       #泛型图形样式
├── tut06_leadingjet.ipynb          #喷注数据排序
├── tut07_initialization.ipynb      #C++11初始化方法对比
└── tut08_rdataframe.ipynb          #高效数据分析框架
              
```



# 📮代码6主要功能说明 #

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

## 3.核心处理流程
### 3.1 使用 `std::sort` 排序
- **目标**：按 `fPt` 降序排列喷注。
- **代码实现**：
  ```cpp
  #include <algorithm> // 必需头文件

  std::sort(jets.begin(), jets.end(), 
      [](const jet& a, const jet& b) { 
          return a.fPt > b.fPt; // 降序排序
      });
---

### 3.2 验证排序结果
**打印排序后的喷注pT**：
    for (const auto& j : jets) {
    std::cout << "Jet pT: " << j.fPt << " GeV/c" << std::endl;
}
---

### 3.3 关键实现要素
- **头文件依赖**：必须包含`<algorithm>`以使用排序功能
- **稳定性**：当多个喷注pT相同时，保持原始相对顺序（稳定排序）
- **复杂度**：时间复杂度为`O(N log N)`，适用于大型数据集
---

## 4.输出验证方法
### 4.1 结果验证步骤
- **1.遍历排序后的喷注集合**
- **2.输出每个喷注的pT值**
- **3.确认输出顺序满足**：
    第一个元素pT值最大
    第二个元素为次大值
    后续元素保持递减顺序
---

## 5. 常见问题与调试
### 5.1 常见错误
- **缺少头文件**：未包含 `<algorithm>` 导致 `std::sort` 编译失败。
- **排序逻辑错误**：Lambda函数中误用 `<` 导致升序排列。
- **空容器**：若 `jets` 为空，需添加空值检查。
---

### 5.2 调试技巧
- 在排序前后打印 `jets` 内容，确认排序逻辑正确性。
- 使用 `assert(jets.size() >= 2)` 确保至少存在两个喷注。
---

## 6.扩展练习

1. **修改排序条件**：尝试按 `fEta` 或 `fPhi` 排序。
2. **处理边界情况**：当 `jets` 为空或仅有一个喷注时，添加异常处理。
3. **性能优化**：探索 `reserve` 预分配内存对大型数据集的影响。
---

## 7.总结
- **核心知识点**：结构体定义、STL容器操作、Lambda函数与排序算法。
- **应用场景**：高能物理实验数据分析（如ATLAS/CMS）、喷注特性研究。
---


<!--by 李诚东-->