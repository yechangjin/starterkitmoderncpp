// ========== Analysis Procedure Description ==========
/*
 * This script uses ROOT RDataFrame to analyze particle track data
 * Main tasks include:
 * 1. Define derived quantities (pt, eta, phi)
 * 2. Data quality checks and filtering
 * 3. Statistical calculations and visualization
 * 4. Data export
 * Requires C++17 standard and ROOT 6.24+
 */

// ========== 1. Data Loading and Validation ==========
{
    // Verify input file integrity
    std::unique_ptr<TFile> branchreader(TFile::Open("./data/rdfexample.root", "READ"));
    auto testtree = static_cast<TTree*>(branchreader->Get("tracktree"));
    testtree->Print();  // Display tree structure
}

// Create RDataFrame instance
ROOT::RDataFrame trackframe("tracktree", "./data/rdfexample.root");

// ========== 2. Define Derived Quantities ==========
auto df_with_branches = trackframe
    // Transverse momentum: pt = √(px² + py²)
    .Define("pt", "sqrt(px*px + py*py)")  
    
    // Pseudorapidity: η = -ln[tan(θ/2)], θ = arctan(√(px²+py²)/pz)
    .Define("eta", "-log(tan(0.5*atan2(sqrt(px*px + py*py), pz)))")  
    
    // Azimuthal angle: φ = atan2(py, px)
    .Define("phi", "atan2(py, px)");       

// Display updated column list
std::cout << "\nUpdated column list:" << std::endl;
for (const auto &col : df_with_branches.GetColumnNames()) {
    std::cout << " • " << col << std::endl;
}

// ========== 3. Data Visualization ==========
auto c1 = new TCanvas("c1", "Analysis Plots", 1200, 800);
c1->Divide(2,2);

// Transverse momentum distribution (1D histogram)
c1->cd(1);
auto h_pt = df_with_branches.Histo1D(
    {"h_pt", "Transverse Momentum;p_{T} [GeV/c];Counts", 100, 0, 10}, 
    "pt"
);
h_pt->DrawCopy();

// η-φ correlation (2D histogram)
c1->cd(2);
auto h_eta_phi = df_with_branches.Histo2D(
    {"h2d", "#eta-#phi Correlation;#eta;#phi [rad]", 
     100, -2, 2, 100, -TMath::Pi(), TMath::Pi()},
    "eta", "phi"
);
h_eta_phi->DrawCopy("COLZ");

// ========== 4. Data Filtering ==========
auto filtered = df_with_branches.Filter(
    "TPCclusters >= 120 && ITSclusters >= 4",  // Quality cuts
    "HighQualityTracks"                        // Filter name for reporting
);

// Plot filtered distributions
c1->cd(3);
auto h_pt_filtered = filtered.Histo1D(
    {"h_pt_filtered", "Filtered pT Distribution;p_{T} [GeV/c];Counts", 100, 0, 10}, 
    "pt"
);
h_pt_filtered->DrawCopy();

// ========== 5. Statistical Analysis ==========
// Direct statistical calculations (no intermediate histograms)
auto pt_mean = filtered.Mean("pt");
auto pt_std = filtered.StdDev("pt");
auto tpc_mean = filtered.Mean("TPCclusters");
auto its_mean = filtered.Mean("ITSclusters");

// Trigger computations and display results
std::cout << "\nStatistical Results:" << std::endl
          << " • ⟨pT⟩ = " << pt_mean.GetValue() << " ± " << pt_std.GetValue() << " GeV/c\n"
          << " • ⟨TPC clusters⟩ = " << tpc_mean.GetValue() << "\n"
          << " • ⟨ITS clusters⟩ = " << its_mean.GetValue() << std::endl;

// ========== 6. Data Export ==========
// Define output columns (excluding px/py/pz)
std::vector<std::string> output_columns = {
    "pt", "eta", "phi", "ITSclusters", "TPCclusters"
};

// Write to CERNBox (replace with actual username)
TString output_path = "/eos/user/your_username/rdf_output.root";
filtered.Snapshot("filteredTracks", output_path, output_columns);

std::cout << "\nAnalysis results saved to: " << output_path << std::endl;

// ========== Multithreading Report ==========
ROOT::RDF::Experimental::AddProgressBar(filtered);
filtered.Report()->Print();
