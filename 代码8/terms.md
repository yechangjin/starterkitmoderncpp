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