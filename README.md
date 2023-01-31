# Class 1 - Monday, January 23

+ Set up C++ environment
+ Compile "Hello World"

### Variables

### Arrays

### Loops

### Conditionals

### Scoping

What is the output of this code?
```cpp
double a = 5;
for (int i = 0; i < 10; i++) {
   double a = 10+i;
}
cout << a << endl;
```

### Functions

## Assignment 1



# Class 2 - Monday, January 30

+ File reading/writing
+ Equationssolvers, Cholesky decomposition


### GitHub Classroom

+ Navigate to `Files Input Output` on GitHub Classroom.
+ Clone the repository into `git clone <HTTPS copied from GitHub>` using the handy-dandy GitHub tool of your choice.
+ Move to and print directory to check (e.g., in PowerShell). `cd`, `dir`
+ Open Local Folder in Visual Studio.
+ Build All! (to make your compiled C++ executable): `Ctrl+Shift+B`

### Demo

+ basic stuff `#include <iostream>`
+ read/write files `#include <ifstream>`
+ help with characters `#include <string>`

### File reading

Dynamically allocated matrix storage and read from file: 

+ Dynamically allocate storage during run time
```cpp
double *A = new double[N*N];
```
+ Read N and filename from external file
```cpp
string filename = "filename.txt";
ifstream infile(filename);
```
+ error checking!!!!!! (especially important for file reading / writing, dividing by zero, etc.)
```cpp
if (infile.fail()) {
   cerr << "error opening file " << filename << endl;
   return -1;
}
```    
+ Move across the rows, but store by the column:
```cpp
for (int r = 0; r < N; r++)
   for (int c = 0; c < N; c++) {
      infile >> A[N*c + r]
   }
}
```
Note: `infile` was defined by the `ifstream`. Similar to `cin`.

+ Need a `delete` for every `new`
```cpp
delete [] A;
```
Note: if pointer to a variable: `delete A` deletes only pointer to beginning of array.
Note: if pointer to an array: `delete [] A` deletes all of memory allocation of the array.

### Alternative reading in

Alternative methods other than `cout` and `argv`:

+ look to your main file!
```cpp
int main (int argc, const char **argv) {
   int N;
         
   // argv[0] program name
   // argv[1] inputs N
   // argv[2] inputs filename
   if (argc < 3) {
      cerr << "insufficient inputs: requires 2 args" << endl;
      return -1
   }
   
   N = atoi(argv[1])
   string infilename = argv[2]
   
   //
   //
}
```

### File writing

Writing to a file `ofstream` v. `ifstream`

+ add a new `argv[3]` with outfilename
+ name your variable: `string outfilename = argv[3]`
+ read in the file: `ofstream(outfilename)`
+ error check!!
```cpp
if (outfile.fail()) {
   cerr << "error opening file " << outfilename << endl;
   return -1;
}
```
+ write out the files:
```cpp
for (int r = 0; r < N; r++)
   for (int c = 0; c < N; c++) {
      outfile << A[N*c + r] << ' ';
   }
}
```  

## Assignment

Cholesky Decomposition:

    Ax = b
    A = L L^T // Factorization
    L L^T x = b
    y = L^T x // Backward substitution
    Ly = b // Forward substitution

+ Cholesky decomposition can be used to solve $Ax = b$
+ where $L$ is a lower triangular matrix (with real and positive diagonal entries and $A$ is symmetric, positive-definite)
+ So $L$ represents the upper and lower triangles where everything else is zero.
+ $Ly = b$ can be solved for $y$ by forward substitution. $Lx = y$ can be solved for $x$ by backward substitution. these two halves of repeated substitutions can be coded easily.
+ Be careful of the many zeroes! This is slow and counts as an operation in C++. You can find solvers to exploit sparsity, symmetry, bandwidth, etc.
+ Direct solvers and iterative solvers depend on the number of dofs. *Topic for future assignment*
+ OpenSees uses third party libraries for its solvers.

\begin{figure}[!htbp]
\centering
\includegraphics[width=1.0\textwidth]{Figs/Assignment 2.PNG}
\caption{\label{fig:frog}Some stuff about Cholesky factorization.}
\end{figure}

## Questions

+ Does the memory reset upon shutdown? yes
+ Statically allocated arrays do not need to be "freed" (i.e., deleted); e.g., `double B[100*100]`. *Can see this in size of compiled program*
+ Can you get the length from the file? Yes, perhaps using a "scan" method, but in C++ you often define the length explicitly, especially if you already know the length of the number of values you want to read. For example, first line of the .txt file can be the length. 
+ `ifstream` Reads file one value by one value and skipping white space.
+ Inputted text file can be relative (e.g., using `../../filename.txt`)
+ `ofstream` makes new file or overrides existing file
+ `.json` is a nice way to read/write files, check out: https://github.com/nlohmann/json *Topic for future session*
+ Many solvers available. 
+ In julia, there's ways to see whether it's column major. may be something similar in c++ (e.g., via `#define`, operator overloading, inline functions, macros).
+ `#include` as you said is used to include a file before the actual compilation. `#define` is used to define a Macro that is replaced by its value just before compilation.
+ Make sure you initialize your data to all zeros (because you never know what's already there to start, could be junk).
+ Initialization
```cpp
// Defaut Initialization of a arbitrary object   
int i{};                // i becomes 0
std::string s{};        // s becomes ""
std::vector<float> v{}; // v becomes an empty vector
double d{};             // d becomes 0.0
```
    
\begin{figure}[!htbp]
\centering
\includegraphics[width=0.3\textwidth]{Figs/Capture.PNG}
\caption{\label{fig:frog}iterative (purple) v. direct (red) solvers.}
\end{figure}
