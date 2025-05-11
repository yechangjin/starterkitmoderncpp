# Period-by-Period Data Comparison Tool

![Example Comparison Plot](period_comparison.png) <!-- by 陆德劲 -->

## Overview <!-- by 陆德劲 -->
This ROOT/C++ tool enables efficient comparison of particle physics datasets across different time periods (LHC runs) through customizable visualization styles. Key features include:

- Dynamic style management using `std::map` and ROOT's `TMap`
- Template-based styling system for consistent visual representation
- Automated multi-period plotting with legend generation
- Support for logarithmic scale visualization

## Installation Guide <!-- by 陆德劲 -->

### Requirements
- [ROOT 6.24+](https://root.cern/install/)
- C++17 compatible compiler (g++ 9+ or clang 10+)
- CMake 3.12+ (recommended)

### Quick Start
```bash
# Clone repository
git clone https://github.com/your-repo/period-comparison.git
cd period-comparison

# Build with CMake
mkdir build && cd build
cmake ..
make -j4

# Run the tool
./period_comparison

# Usage Documentation <!-- by 陆德劲 -->
#1. Defining Plot Styles

The PlotStyle class encapsulates visual properties for datasets:
/**
 * @class PlotStyle
 * @brief Encapsulates visual style properties for dataset plotting
 */
class PlotStyle : public TObject {
    Color_t fColor;
    Style_t fMarker;
    
public:
    PlotStyle() : TObject(), fColor(0), fMarker(0) {}
    PlotStyle(Color_t color, Style_t marker) 
        : TObject(), fColor(color), fMarker(marker) {}
    
    // Setter methods
    void SetColor(Color_t color) { fColor = color; }
    void SetMarker(Style_t marker) { fMarker = marker; }
    
    // Getter methods
    Color_t GetColor() const { return fColor; }
    Style_t GetMarker() const { return fMarker; }
    
    /**
     * @brief Applies style to a ROOT graphics object
     * @tparam T Graphics object type (TH1, TGraph, etc.)
     * @param obj Pointer to graphics object
     */
    template<typename T>
    void SetStyle(T* obj) const {
        obj->SetMarkerColor(fColor);
        obj->SetLineColor(fColor);
        obj->SetMarkerStyle(fMarker);
    }
};
#2. Configuring Dataset Styles
Configure styles using associative containers:
#// Using std::map
std::map<std::string, PlotStyle> styleMap = {
    {"LHC17h", {kRed, 24}},    // Red, triangle-up
    {"LHC17i", {kBlue, 25}},   // Blue, triangle-down
    {"LHC17k", {kGreen, 26}},  // Green, square
    {"LHC17m", {kViolet, 27}}  // Violet, circle
};

#// Alternative using ROOT's TMap
TMap styleTMap;
styleTMap.Add(new TObjString("LHC17h"), new PlotStyle(kRed, 24));
styleTMap.Add(new TObjString("LHC17i"), new PlotStyle(kBlue, 25));
styleTMap.Add(new TObjString("LHC17k"), new PlotStyle(kGreen, 26));
styleTMap.Add(new TObjString("LHC17m"), new PlotStyle(kViolet, 27));

#3. Generating Comparison Plots
Complete workflow example:
#// (1). Load histograms
std::map<std::string, TH1*> periodHists;
TFile* inputFile = TFile::Open("period_data.root");

for (auto&& key : *inputFile->GetListOfKeys()) {
    std::string period = key->GetName();
    TH1* hist = dynamic_cast<TH1*>(inputFile->Get(Form("%s/hpt", period.c_str())));
    
    if (hist) {
        hist->SetDirectory(nullptr);
        
        // Apply style
        auto styleIter = styleMap.find(period);
        if (styleIter != styleMap.end()) {
            styleIter->second.SetStyle(hist);
        }
        
        periodHists[period] = hist;
    }
}

#//(2) . Create canvas
TCanvas* canvas = new TCanvas("comparison", "Period Comparison", 1200, 800);
canvas->SetLogy();

#// (3). Create frame histogram
TH1F* frame = new TH1F("frame", ";p_{T} (GeV/c);dN/dp_{T}", 100, 0, 100);
frame->SetStats(0);
frame->GetYaxis()->SetRangeUser(1e-10, 100);
frame->Draw();

// 4. Draw all period histograms
TLegend* legend = new TLegend(0.7, 0.6, 0.9, 0.9);
legend->SetBorderSize(0);

for (auto& [period, hist] : periodHists) {
    hist->Draw("EP SAME");
    legend->AddEntry(hist, period.c_str(), "lep");
}

legend->Draw();
canvas->Update();
