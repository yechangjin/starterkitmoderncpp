# tut07_initialization

# How to Run This Project

Below is a detailed guide on how to run the `starterkitmoderncpp` project.

## I. Environment Requirements
1. **C++ Compiler**
   - A compiler that supports C++11 or higher is required, such as `g++` or `clang++`.
2. **ROOT Framework**
   - The Jupyter Notebook of this project uses the ROOT C++ kernel.
   - Please install the ROOT framework to support execution.
3. **Jupyter Notebook**
   - Jupyter Notebook must be installed to run `.ipynb` files.
4. **Python and Related Dependencies**
   - Ensure that Python and the dependencies required for Jupyter Notebook are installed.
---


## II. Installation Steps
### 1. Clone the Project Locally
Run the following command to clone the project:
```bash
git clone https://github.com/qqx123456/starterkitmoderncpp.git 
cd starterkitmoderncpp
```
### 2. Install ROOT Framework
If you have not installed the ROOT framework, please refer to the [official documentation](https://root.cern/install/) for installation.
Make sure the `root` command can run in the terminal.

### 3. Install Jupyter Notebook
If you have not installed Jupyter Notebook, please refer to the [official documentation](https://jupyter.org/install) for installation.
You can install it with the following command:
```bash
pip install notebook
```

### 4. Install ROOT C++ Kernel
If you have not installed the ROOT C++ kernel, please refer to the [official documentation](https://root.cern/install/all-in-one/) for installation.
Run the following command to add the ROOT C++ kernel for Jupyter Notebook:
```bash
conda install -c conda-forge root
```


### Run the Project
1. Start Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
2. Open the project file
   Open Jupyter Notebook in the browser:
   Find and open the `.ipynb` file in the project, and click to open. For example: `tut07_initialization.ipynb`.

3. Select the kernel
   In the top menu of Jupyter Notebook, select Kernel, then select `Change Kernel`, and choose the `Root C++` kernel.

4. Run the code cells in order
   In the Notebook, run each code cell in order and observe the results.



## III. Precautions
1. Check Dependencies
   - Ensure that all dependencies are installed, such as the ROOT framework, Jupyter Notebook, Python, etc.
   - Ensure that the ROOT environment variable is present in the runtime environment.
2. Kernel Compatibility
   - If you cannot run the code in Jupyter Notebook, please check whether the correct kernel has been selected, such as the `Root C++` kernel.

3. Debugging Issues
   - If you encounter any issues, it is recommended to check the relevant error messages and fix them according to the prompts, or raise an issue in [GitHub issues](https://github.com/qqx123456/starterkitmoderncpp/issues).




## Explanation of `tut07_initialization.ipynb`

`tut07_initialization.ipynb` demonstrates various methods and techniques for object initialization in modern C++11, including the differences between initialization with `()` and `{}`, the initialization methods for POD (Plain Old Data) objects, and the use of initializer lists to dynamically store data. This tutorial is suitable for developers who wish to gain an in-depth understanding of C++ initialization mechanisms.



# I. Object Initialization in C++11

## The Difference Between () and {}

Objects can be initialized with normal parentheses `()` and curly braces `{}`. Curly braces have some restrictions that may help make the code more robust.

## Core Differences
- **Parentheses `()`**  
  Allows implicit type conversion, but may lead to narrowing conversions (e.g., `double` → `int` causing precision loss).
- **Braces `{}`**  
  Strictly prohibits narrowing conversions, enhancing code robustness.

## Example Code
```cpp
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



Original document analysis:
# C++ Class `SimpleEvent` Code Explanation

## Class Definition

The `SimpleEvent` class includes the following member variables:
- `int fNtrack`: Used to store the number of tracks as an integer.
- `double fVz`: Used to store the vertical position as a double-precision floating point number.

## Constructors

  * **Default Constructor**
  ```cpp
  SimpleEvent() : fNtrack(0), fVz(DBL_MIN) {}
  ```

## Class Definition
```cpp
class SimpleEvent {
    int fhttrack;
    double fvz;

public:
    // Constructor and method definitions
};
    SimpleEvent() : fhttrack(0), fvz(OBL_PUN) {} // Initialize fNtrack to 0, fVz to DBL_MIN (the smallest value of a double-precision floating point number). This initialization method is used when the object is not provided with explicit initialization parameters.
    SimpleEvent(int ntrack, double vz) : fhttrack(ntrack), fvz(vz) {} // Initialize fNtrack and fVz based on the input parameters ntrack and vz. Allows the object to be assigned specific values at creation.

    void SetNTrack(int ntrack) { fNtrack = ntrack; } // Used to set the value of fNtrack, where ntrack is the new value to be set.
    void SetVz(double vz) { fVz = vz; } // Used to set the value of fVz, where vz is the new value to be set.

    int GetNTrack() const { return fNtrack; } // Used to get the value of fNtrack, return type is int, and the function is declared as const, indicating that calling this function will not modify the state of the object.
    double GetVz() const { return fVz; } // Used to get the value of fVz, return type is double, also declared as const.

    
Initialize objects with (). Initialize objects with parentheses. One case uses a narrowing implicit type conversion (the value has lower precision than the parameter in the constructor).
SimpleEvent nb1(10, 0.5);
SimpleEvent nb2(5., 10.);    // Narrowing. Precision loss due to conversion from double to int.
// What about the other direction: Implicit cast with "gain" in precision.
SimpleEvent nb3{15, 20};

Initialize objects with {}. One case uses a narrowing implicit type conversion (the value has lower precision than the parameter in the constructor). What happens in the narrowing case?
SimpleEvent cb1{10, 0.5};
SimpleEvent cb2{5., 10.};    // Narrowing. Precision loss due to conversion from double to int.
// "What about the other direction: Implicit cast with "gain" in precision
SimpleEvent cb3{15, 20};
```



## II. POD Objects
Plain Old Data structure (POD) consists only of a set of attributes — no object-oriented principles (such as encapsulation, inheritance, etc.) apply. In C++, POD objects are represented by structs. POD objects can be useful if the amount of data it holds and the functionality of the object is limited.

```cpp
struct Style {  // Define a struct named Style to store style information
    Color_t fColor;   // Member variable fColor, type Color_t, used to store color information
    Style_t fMarker;  // Member variable fMarker, type Style_t, used to store marker style information

    template<typename t>    // Define a template function SetStyle, accepting a pointer parameter object of any type t
    void SetStyle(t * object) {
        object->SetMarkerColor(fColor);   // Call the object's SetMarkerColor method to set the marker color to fColor
        object->SetMarkerStyle(fMarker);    // Call the object's SetMarkerStyle method to set the marker style to fMarker
        object->SetLineColor(fColor);   // Call the object's SetLineColor method to set the line color to fColor
    }
};


Does this struct look familiar to you?
Construct POD object with ().

Style s1(kRed, 24);    // In Style s1(kRed, 24), kRed is the color value, 24 is the marker style value
// This method may allow implicit type conversion

Initialize POD object with {}.

Style s2{kBlue, 25};    // In Style s2{kBlue, 25}, kBlue is the color value, 25 is the marker style value
// This method is stricter and may prevent precision loss due to implicit type conversion
```



## III. Initializer Lists

In the example above, the constructor is invoked using curly braces with exactly two parameters. What if the number of parameters to store needs to be dynamic?

```cpp
template<typename T>
class Container {  // Defines a template class Container for storing data of any type
    std::vector<T> fData;  // Member variable fData of type std::vector, used to store data
public:
    Container() : fData() {}  // Default constructor, initializes fData as empty

    // Constructor using an initializer list, accepting a std::initializer_list as a parameter
    Container(std::initializer_list<T> in) : fData() {
        for(decltype(in.begin()) it = in.begin(); it != in.end(); it++) { 
            fData.emplace_back(*it);  // Adds elements from the initializer list to fData
        }
    }

    // Returns a reference to fData
    const std::vector<T>& Data() const { return fData; }
};

Create a container with several values and print it.

```cpp
Container<int> test1{1, 2, 3, 4, 5}; // Initializes Container object test1 with integers 1 to 5 using curly braces
Container<int> test2 = {6, 7, 8, 9, 10}; // Initializes Container object test2 with integers 6 to 10 using the equals sign and curly braces
std::cout << "Container1: " << test1 << "; Container2: " << test2 << std::endl; // Prints contents of test1 and test2

Initializer lists must always contain objects of the same type!  



