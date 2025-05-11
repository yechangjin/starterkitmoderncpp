```markdown
# ROOT C++ Data Analysis Tutorial Collection

A set of tutorials based on the ROOT framework and modern C++ features for data analysis, covering smart pointers, generic programming, data visualization, and other core skills.

![ROOT Framework Logo](https://root.cern/img/logos/ROOT_Logo/ROOT_Logo_vertical.png  )

## 📋 Project Overview
- **Core Framework**: ROOT (a high-energy physics data analysis tool developed by CERN)
- **Language Features**: C++11/14 (smart pointers, lambda expressions, initializer lists, etc.)
- **Tutorial Topics**: 8 independent modules covering file operations, data sorting, style management, and other scenarios
- **Data Format**: Uses `.root` binary files to store histograms and tree-structured data

## 🛠️ Environment Setup
### Project Cloning
Run the following command to clone the project:
```bash
git clone https://github.com/yechangjin/starterkitmoderncpp.git 
cd starterkitmoderncpp
```
### Dependency Installation
1. **ROOT Framework** ([Official Installation Guide](https://root.cern/install/  ))
   ```bash
   # Linux Example (Ubuntu)
   wget https://root.cern/download/root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz  
   tar -xzvf root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz
   source root/bin/thisroot.sh
   ```
2. **Compiler**: GCC 9+ or Clang 10+ (must support C++14)
3. **Editor**: VS Code + Extensions
   - [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools  )
   - [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter  )

### Data Preparation
```text
Project Root Directory/
├── data/                  # Directory for all .root data files
│   ├── hsimple.root       # Sample histogram data
│   └── rdfexample.root    # RDataFrame test data
│                          # Tutorial code files
├── tut01_sharedptr.ipynb          # Shared pointer reference counting
├── tut02_filereader.ipynb         # Smart pointer management for ROOT files
├── tut03_findhists.ipynb          # Dynamic histogram lookup
├── tut04_periodcomparison.ipynb   # Multi-experiment period data comparison
├── tut05_genericstyles.ipynb      # Generic graphics styles
├── tut06_leadingjet.ipynb         # Jet data sorting
├── tut07_initialization.ipynb     # Comparison of C++11 initialization methods
└── tut08_rdataframe.ipynb         # Efficient data analysis framework
              
```

## 🚀 Quick Start

### Jupyter Mode (Recommended)
```bash
# Launch Jupyter with ROOT kernel
root --notebook
```
- Open the `.ipynb` file in a browser
- Select the kernel as **ROOT C++**
- Execute each Cell one by one and observe the output

![Jupyter Interface Example](https://jupyter.org/assets/homepage-main.png  )

### Command Line Compilation
```bash
# Example: Compile tut03 file
g++ tutorials/tut03_findhists.C -o output \
    $(root-config --cflags --libs --glibs)
./output
```

## 📚 Tutorial Catalog

| File                      | Core Content                                          | Key Technical Points                  |
|--------------------------|--------------------------------------------------------|---------------------------------------|
| `tut01_sharedptr`        | Shared pointer reference counting                      | `std::shared_ptr`                     |
| `tut02_filereader`       | Smart pointer management for ROOT files                | `std::unique_ptr<TFile>`              |
| `tut03_findhists`        | Dynamic histogram lookup                               | Traversal of `TObjArray`              |
| `tut04_periodcomparison` | Multi-experiment period data comparison                | Style management with `std::map`      |
| `tut05_genericstyles`    | Generic graphics styles                                | C++14 lambda closures                 |
| `tut06_leadingjet`       | Jet data sorting                                       | `std::sort` + custom comparator       |
| `tut07_initialization`   | Comparison of C++11 initialization methods             | List initialization `{}`              |
| `tut08_rdataframe`       | Efficient data analysis framework                      | ROOT::RDataFrame                      |
<!--by ningguokang-->



 **`tut01_sharedptr`**:
# Shared-Pointer Usage

## Payload Class

**Purpose**: As an example class for shared resources, used to visualize the calls of constructors and destructors.

### Member Functions

- **Constructor**:  
  Called when a `Payload` object is created, outputs a prompt message.

- **Copy Constructor**:  
  Called when initializing a new object with an existing `Payload` object, outputs a prompt message.

- **Assignment Operator**:  
  Called when assigning a `Payload` object to another existing object, outputs a prompt message.

- **Destructor**:  
  Called when the `Payload` object's lifetime ends, outputs a prompt message.

## Client Class

**Member Variables**:  
- `std::shared_ptr<Payload> fPayload`: Manages the shared pointer to a `Payload` object.

### Constructors

- **Default Constructor**:  
  Called when creating a `Client` object, initializes the shared pointer, and outputs a prompt message.

- **Parameterized Constructor**:  
  Accepts a `Payload` pointer, encapsulates it into a shared pointer, and outputs the current number of clients accessing the `Payload`.

### Copy Constructor

- **Copy Constructor**:  
  Called when initializing a new object with an existing `Client` object, shares the same `Payload` object, and outputs the current number of clients accessing the `Payload`.

### Assignment Operator

- **Assignment Operator**:  
  Called when assigning a `Client` object to another existing object, updates the shared pointer's target, and outputs the current number of clients accessing the `Payload`.

### Destructor

- **Destructor**:  
  Called when the `Client` object's lifetime ends, outputs the current number of clients accessing the `Payload` (before deletion).

## Main Function (`main`)

1. **Create a Base Client Object**:  
   Create a base client object `base` and allocate a new `Payload` resource for it. At this point, the reference count is **1**.

2. **Create a Vector of Client Objects**:  
   Create a vector of 10 other client objects called `others`, each created by copying `base`. They share the same `Payload` resource, and the reference count gradually increases.

3. **Delete Client Objects in the Vector**:  
   Delete all client objects in the vector. Each deletion reduces the reference count. However, the `Payload` resource still exists because `base` still points to it.

4. **Delete the Base Client Object**:  
   Finally, delete the base client object `base`. At this point, the reference count becomes **0**, the `Payload` object is automatically deleted, and the resource is released.

<!--yechangjin-->






 **`tut02_filereader`**:

hello teacher:
This code author contribution:

File Handling with Smart Pointers
File Handling and Smart Pointers
This paragraph introduces the role of smart pointers in file handling, explaining how they control the lifespan of file objects and prevent accidental connections to files. When smart pointers go out of scope, they automatically call the destructor of the object (here, the TFile destructor) to close the file.

File Reading Example
 Provides a code example demonstrating how to use smart pointers to read a file and retrieve objects. The example utilizes std::unique_ptr to manage the file object's lifecycle, explaining how the file is automatically closed when the scope ends and how to disconnect the retrieved object from the file.
 
Plotting Example
 Offers another code example showing how to draw a histogram object read from a file in memory.
Checking if the File Is Still Open
Author contribution: Presents a simple code snippet to check whether the file remains open by verifying if the gFile pointer is directed at a valid memory address.
<!--chengshichao-->





 **`tut03_findhists`**:

---

### Dynamic Histogram Lookup (tut03)
```cpp
TH1* findHist(const char* filename, const char* histtag) {
    TH1* hist = nullptr;
    std::unique_ptr<TFile> reader(TFile::Open(filename, "READ")); // Smart pointer for file management
    
    // Check if the file is valid
    if (!reader || reader->IsZombie()) {
        std::cerr << "Error: Failed to open file " << filename << std::endl;
        return nullptr;
    }

    // Get the object corresponding to the first key (assumed to be TObjArray)
    TKey* firstKey = static_cast<TKey*>(reader->GetListOfKeys()->At(0));
    TObjArray* histList = dynamic_cast<TObjArray*>(firstKey->ReadObj());
    
    // Iterate through the object array to find the matching histogram
    if (histList) {
        for (auto obj : *histList) {
            TH1* candidate = dynamic_cast<TH1*>(obj); // Dynamic type conversion
            if (candidate && std::string(candidate->GetName()).find(histtag) != std::string::npos) {
                hist = candidate;
                hist->SetDirectory(nullptr); // Disconnect from the file
                break;
            }
        }
    }
    return hist;
}
```

---

### Code Explanation
| Key Step                  | Technical Point                                                                 |
|--------------------------|----------------------------------------------------------------------|
| **Smart pointer for file management**       | `std::unique_ptr<TFile>` ensures automatic file closure and prevents resource leaks              |
| **Dynamic type conversion**           | `dynamic_cast<TObjArray*>` and `dynamic_cast<TH1*>` ensure type safety      |
| **Histogram name matching**         | `std::string::find` implements fuzzy matching (e.g., finding histograms with names containing `"hPt"`)  |
| **Disconnect from file**           | `SetDirectory(nullptr)` prevents the histogram from being destroyed when the file is closed                  |

---

### Usage Example
```cpp
// Find and plot a histogram
TH1* hPt = findHist("./data/10111000101_trackqa.root", "hPt");
if (hPt) {
    TCanvas* canvas = new TCanvas("canvas", "Plot", 800, 600);
    hPt->Draw();
    canvas->SaveAs("hPt_plot.png"); // Export as an image
} else {
    std::cerr << "Error: Histogram hPt not found!" << std::endl;
}
```

---

### Expected Output
1. When the histogram is successfully found:  
   ![hPt_plot.png](https://via.placeholder.com/800x600/EEE/000?text=hPt+Histogram  )
2. When it fails, terminal output:  
   ```text
   Error: Histogram hPt not found!
   ```

---

## ⚠️ Common Issues

### Compilation Error: ROOT header files not found
```bash
# Check environment variables
echo $ROOTSYS
# Reconfigure the compilation command
$(root-config --cflags --libs)
```

### Runtime Data File Missing
```text
Error message: Error in <TFile::TFile>: file ./data/xxx.root does not exist
Solution:
1. Check the directory structure of data/
2. Download sample data from the project repository
```
<!--by ningguokang-->





 **`tut04_periodcomparison`**:
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

<!--ludejin-->






**`tut05_genericstyles`**:
### Running the Code  
1. **Start the ROOT Environment**:  
   ```bash
   root
   ```  

2. **Execute Code in the ROOT Interactive Environment**:  
   ```cpp
   // Create style closure
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

   // Create specific styles
   auto graphStyle = createStyle(kRed, 24);
   auto histoStyle = createStyle(kBlue, 25);

   // Apply styles
   graphStyle(*testgraph);
   histoStyle(*testhisto);

   // Draw the plot
   TCanvas *c1 = new TCanvas("c1", "Plot Demo", 800, 600);
   testhisto->Draw("hist");
   testgraph->Draw("P same");
   c1->Update();
   ```  

3. **Or Run Directly in Jupyter Notebook**:  
   Switch to the ROOT C++ kernel in Jupyter Notebook:  
   ![Switch Kernel](switch_kernel.png)  
   Then execute the code.  

<!--lianghongjun-->







 **`tut06_leadingjet`**:
## 1. Leading Jets Analysis Tutorial in Events
**Purpose**: In particle collision events containing multiple jets, identify and extract the two jets with the highest transverse momentum (pT). This guide describes the complete data processing workflow, applicable to physics data analysis within the ROOT framework.
**Application** Scenarios: Particle physics data analysis, beginners to the ROOT framework, jet property processing.
---

## 2. Code Structure Analysis📦
### 2.1 Jet Data Structure Definition and Design
- **`struct jet`: Stores physical properties of jets**:
    - `fPt` (transverse momentum), `fEta` (pseudorapidity), `fPhi` (azimuthal angle), 
    - `fNConstituents` (number of constituents).
---

## 2.2 Jet Data Initialization
- **`std::vector<jet> jets`**: Example data contains 5 jets, values are for demonstration only.
- Data initialized in `{pT, eta, phi, n}` format.
---

<!--lichengdong-->






 **`tut07_initialization`**:
class SimpleEvent {
    int fhtrack;     // track code
    double fvz;      // vertex coordinate

public:
    SimpleEvent(int ntrack, double vz) : fhtrack(ntrack), fvz(vz) {}
};

// Legal initialization
SimpleEvent nb1(10, 0.5);    // Parentheses, implicit int → double
SimpleEvent cb1{10, 0.5};    // Braces, legal

// Illegal initialization (narrowing conversion)
// SimpleEvent nb2(5.0, 10);  // Warning: double → int precision loss (compiler might pass)
// SimpleEvent cb2{5.0, 10};  // Error: braces prohibit narrowing (compilation fails)

Summary of rules
1. Prefer using {} for initialization to avoid unexpected narrowing.
2. If implicit conversion is needed, use () for initialization, but ensure type compatibility is explicitly compatible.

<!--suguoquan-->






 **`tut08_rdataframe`**:
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

<!--xuzhifan-->
```



