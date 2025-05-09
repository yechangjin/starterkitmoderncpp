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

Below is the core code example for **`tut03_findhists`**:

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
```