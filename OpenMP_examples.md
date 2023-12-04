# OpenMP offload code examples
Examples of OpenMP offload code are not plentiful.  Some places I've found to get example code.

### OpenMP organization
The OpenMP organization maintains examples along with the standard
* https://github.com/OpenMP/Examples  (see the tags for different standard versions)
* Links to the Examples documents are on the [Specifications](https://www.openmp.org/specifications/) page.

### Books
* [Programming Your GPU with OpenMP: Performance Portability for GPUs](https://direct.mit.edu/books/monograph/5685/Programming-Your-GPU-with-OpenMPPerformance)   - An excellent guide to OpenMP offload programming.
   * Associated website: https://ompgpu.com/

### Tutorials
* Tutorial files: https://github.com/vlkale/OpenMP-tutorial
* OpenMP 4.5 examples: https://github.com/colleeneb/openmp45_examples
* From Oak Ridge National Laboratory 
  * Part 1: [Basics of Offload](https://www.olcf.ornl.gov/calendar/introduction-to-openmp-offload-part-1-basics-of-offload-2/)
  * Part 2: [Optimization and Data Movement](https://www.olcf.ornl.gov/https/wwwolcfornlgov/calendar/introduction-to-openmp-offload-part-2-optimization-and-data-management-2/)

### Calling external libraries
* Calling nvblas, cublas, and MKL: https://github.com/colleeneb/openmp_offload_and_blas


### Negative examples
* Examples with memory access issues: https://github.com/RWTH-HPC/DRACC.git

### LLVM
* OpenMP tests directory in clang: https://github.com/llvm/llvm-project/tree/main/clang/test/OpenMP
* libomptarget tests in openmp: https://github.com/llvm/llvm-project/tree/main/openmp/libomptarget/test


### Examples
* Large collection of kernels and small codes in CUDA/HIP/Sycl/OpenMP: https://github.com/zjin-lcf/HeCBench
