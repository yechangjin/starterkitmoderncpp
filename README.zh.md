// ========== 分析步骤说明 ==========
/*
 * 本脚本使用ROOT RDataFrame分析粒子轨迹数据
 * 主要完成以下任务：
 * 1. 定义物理量分支(pt, eta, phi)
 * 2. 数据质量检查与筛选
 * 3. 统计计算与可视化
 * 4. 数据导出
 * 使用C++17标准编译，需要ROOT 6.24+
 */

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

// ========== 3. 数据可视化 ==========
auto c1 = new TCanvas("c1", "Analysis Plots", 1200, 800);
c1->Divide(2,2);

// 横向动量分布 (1D直方图)
c1->cd(1);
auto h_pt = df_with_branches.Histo1D(
    {"h_pt", "Transverse Momentum;p_{T} [GeV/c];Counts", 100, 0, 10}, 
    "pt"
);
h_pt->DrawCopy();

// η-φ 二维分布 (2D直方图)
c1->cd(2);
auto h_eta_phi = df_with_branches.Histo2D(
    {"h2d", "#eta-#phi Correlation;#eta;#phi [rad]", 
     100, -2, 2, 100, -TMath::Pi(), TMath::Pi()},
    "eta", "phi"
);
h_eta_phi->DrawCopy("COLZ");

// ========== 4. 数据筛选 ==========
auto filtered = df_with_branches.Filter(
    "TPCclusters >= 120 && ITSclusters >= 4",  // 筛选条件
    "HighQualityTracks"                        // 过滤器名称(用于报告)
);

// 绘制筛选后分布
c1->cd(3);
auto h_pt_filtered = filtered.Histo1D(
    {"h_pt_filtered", "Filtered pT;p_{T} [GeV/c];Counts", 100, 0, 10}, 
    "pt"
);
h_pt_filtered->DrawCopy();

// ========== 5. 统计计算 ==========
// 直接计算统计量(不通过直方图)
auto pt_mean = filtered.Mean("pt");
auto pt_std = filtered.StdDev("pt");
auto tpc_mean = filtered.Mean("TPCclusters");
auto its_mean = filtered.Mean("ITSclusters");

// 触发计算并获取结果
std::cout << "\n统计结果:" << std::endl
          << " • ⟨pT⟩ = " << pt_mean.GetValue() << " ± " << pt_std.GetValue() << " GeV/c\n"
          << " • ⟨TPC clusters⟩ = " << tpc_mean.GetValue() << "\n"
          << " • ⟨ITS clusters⟩ = " << its_mean.GetValue() << std::endl;

// ========== 6. 数据导出 ==========
// 定义输出列（排除px/py/pz）
std::vector<std::string> output_columns = {
    "pt", "eta", "phi", "ITSclusters", "TPCclusters"
};

// 写入CERNBox（替换实际用户名）
TString output_path = "/eos/user/your_username/rdf_output.root";
filtered.Snapshot("filteredTracks", output_path, output_columns);

std::cout << "\n分析结果已保存至: " << output_path << std::endl;

// ========== 多线程报告 ==========
ROOT::RDF::Experimental::AddProgressBar(filtered);
filtered.Report()->Print();