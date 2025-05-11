<!--by lichengdong-->

```markdown
# ROOT C++ Data Analysis Tutorial Collection

A set of tutorials based on the ROOT framework and modern C++ features for data analysis, covering smart pointers, generic programming, data visualization, and other core skills.

![ROOT Framework Logo](https://root.cern/img/logos/ROOT_Logo/ROOT_Logo_vertical.png  )

## 📋 Project Overview
- **Core Framework**: ROOT (a high-energy physics data analysis tool developed by CERN)
- **Language Features**: C++11/14 (smart pointers, lambda expressions, initializer lists, etc.)
- **Tutorial Topics**: 8 independent modules covering file operations, data sorting, style management, and other scenarios
- **Data Format**: Uses `.root` binary files to store histograms and tree-structured data

## 🛠 Environment Setup

### Dependency Installation
1. **ROOT Framework** ([Official Installation Guide](https://root.cern/install/  ))
   ```bash
   # Linux Example (Ubuntu)
   wget https://root.cern/download/root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz  
   tar -xzvf root_v6.26.00.Linux-ubuntu20-x86_64-gcc9.3.tar.gz
   source root/bin/thisroot.sh
   ```
2. **Compiler**: GCC 9+ or Clang 10+ (must support C++14)
3. **Editor**: VS Code + Extensions
   - [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools  )
   - [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter  )

### Data Preparation
```text
Project Root Directory/
├── data/                  # Directory for all .root data files
│   ├── hsimple.root       # Sample histogram data
│   └── rdfexample.root    # RDataFrame test data
│                          # Tutorial code files
├── tut01_sharedptr.ipynb          # Shared pointer reference counting
├── tut02_filereader.ipynb         # Smart pointer management for ROOT files
├── tut03_findhists.ipynb          # Dynamic histogram lookup
├── tut04_periodcomparison.ipynb   # Multi-experiment period data comparison
├── tut05_genericstyles.ipynb      # Generic graphics styles
├── tut06_leadingjet.ipynb         # Jet data sort
```

# 📮 Code 6 Main Function Description #

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

## 3. Core Processing Workflow
### 3.1 Sorting with `std::sort`
- **Purpose**: Sort jets in descending order of `fPt`.
- **Implementation**:
    ```cpp
  #include <algorithm> // 必需头文件

  std::sort(jets.begin(), jets.end(), 
      [](const jet& a, const jet& b) { 
          return a.fPt > b.fPt; // 降序排序

      });
---

## 3.2 Validating Sorted Results:
**Print sorted jet pT values**:
        for (const auto& j : jets) {
        std::cout << "Jet pT: " << j.fPt << " GeV/c" << std::endl;
}
---

### 3.3 Key Implementation Elements
- **Header Dependency**: `<algorithm>` required for sorting.
- **Stability**: Preserves relative order of jets with equal pT (stable sort).
- **Complexity**: Time complexity `O(N log N)`, suitable for large datasets.
---

## 4. Output Validation Methodology
### 4.1 Validation Steps
- **Iterate through sorted jets**
- **Print pT values**
- **Verify ordering:**
-   First element has maximum pT
-   Second element has second-highest pT
-   Subsequent elements follow descending order
---

## 5. Common Issues & Debugging
### 5.1 Common Errors
- **Missing header:** Compilation failure due to omitting <algorithm>.
- **Incorrect sorting logic:** Accidental use of `<` instead of `>` for descending order.
- **Empty container:** Add a check for empty containers (e.g., `if (jets.empty())`) to avoid undefined behavior.
---

### 5.2 Debugging Techniques
-   Print jets contents before/after sorting to verify logic.
-   Use assert(jets.size() >= 2) to ensure ≥2 jets exist.
---

### 6. Extended Exercises
1. **Modify sorting criteria**: Sort by `fEta` or `fPhi`.
2. **Handle edge cases:** Add exception handling for empty/single-jet containers.
3. **Performance optimization:** Explore impact of `reserve`() for memory pre-allocation on large datasets.
---

## 7. Summary
- **Core concepts**: Struct definition, STL container operations, lambda functions, sorting algorithms.
- **Applications**: High-energy physics experiment data analysis (e.g., ATLAS/CMS), jet property studies.
---

<!--by lichengdong-->