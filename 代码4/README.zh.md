# 周期数据对比分析工具

![周期对比示例图](period_comparison.png) <!-- by 陆德劲 -->

## 项目概述 <!-- by 陆德劲 -->
本工具基于ROOT/C++开发，专为粒子物理实验数据分析设计，提供跨周期(LHC运行周期)数据对比可视化解决方案。核心功能包括：

- 基于关联容器(`std::map`/`TMap`)的样式管理系统
- 模板化的统一绘图风格控制
- 自动化多周期对比图表生成
- 支持对数坐标可视化

## 安装指南 <!-- by 陆德劲 -->

### 系统要求
- [ROOT 6.24+](https://root.cern/install/) 数据分析框架
- 支持C++17的编译器(g++ 9+或clang 10+)
- 推荐使用CMake 3.12+构建系统

### 快速开始
```bash
# 克隆代码库
git clone https://github.com/your-repo/period-comparison.git
cd period-comparison

# 编译项目
mkdir build && cd build
cmake ..
make -j4

# 运行分析工具
./period_comparison

#使用手册 <!-- by Da陆德劲vid -->
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
#3. 生成对比图表
完整工作流程示例：
// 1. 加载周期数据
std::map<std::string, TH1*> periodHists;
TFile* inputFile = TFile::Open("period_data.root");

for (auto&& key : *inputFile->GetListOfKeys()) {
    std::string period = key->GetName();
    TH1* hist = dynamic_cast<TH1*>(inputFile->Get(Form("%s/hpt", period.c_str())));
    
    if (hist) {
        hist->SetDirectory(nullptr);  // 确保直方图持久化
        
        // 应用预设样式
        auto styleIter = styleMap.find(period);
        if (styleIter != styleMap.end()) {
            styleIter->second.SetStyle(hist);
        }
        
        periodHists[period] = hist;
    }
}

// 2. 创建画布
TCanvas* canvas = new TCanvas("comparison", "周期数据对比", 1200, 800);
canvas->SetLogy();  // 设置对数坐标

// 3. 创建参考坐标系
TH1F* frame = new TH1F("frame", ";p_{T} (GeV/c);dN/dp_{T}", 100, 0, 100);
frame->SetStats(0);  // 关闭统计信息框
frame->GetYaxis()->SetRangeUser(1e-10, 100);
frame->Draw();

// 4. 绘制各周期数据
TLegend* legend = new TLegend(0.7, 0.6, 0.9, 0.9);
legend->SetBorderSize(0);  // 无边框

for (auto& [period, hist] : periodHists) {
    hist->Draw("EP SAME");  // 保持相同画布
    legend->AddEntry(hist, period.c_str(), "lep");  // 添加图例项
}

legend->Draw();
canvas->Update();  // 刷新显示

#最佳实践
1.样式规范：保持相关分析项目间的颜色方案一致性

2.容器选择：纯C++项目推荐std::map，深度ROOT集成项目建议TMap

3.内存管理：对需要持久化的直方图务必设置SetDirectory(nullptr)

4.异常处理：生产环境应添加文件操作和类型转换的健壮性检查

#常见问题
1.样式未生效：检查周期名称拼写(区分大小写)

2.图例重叠：调整图例位置或使用SetNColumns(2)分栏显示

3.比例失调：对比前确认直方图归一化处理

### 项目术语表（中英文对照）

| 中文术语 | 英文术语                     |                说明                           |
| 关联容器 | Associative Container       | 用于存储键值对的数据结构，如`std::map`和`TMap`。 |
| 绘图样式 | Plot Style                  | 定义图形的颜色、标记等属性的集       合。        |
| 数据分布 | Data Distribution           | 数据的分布情况，例如不同周期的数据。             |
| 周期比较 | Period-by-Period Comparison | 对不同周期的数据进行比较分析。                   |
| 直方图   | Histogram                   | 一种图形，用于展示数据的分布情况。               |