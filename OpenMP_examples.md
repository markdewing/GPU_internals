# OpenMP offload code examples
Examples of OpenMP offload code are not plentiful.  Some places I've found to get example code.

### OpenMP organization
The OpenMP organization maintains examples along with the standard
* https://github.com/OpenMP/Examples  (see the tags for different standard versions)
* Links to the Examples documents are on the [Specifications](https://www.openmp.org/specifications/) page.

### Calling external libraries
* Calling nvblas, cublas, and MKL: https://github.com/colleeneb/openmp_offload_and_blas


### Negative examples
* Examples with memory access issues: https://github.com/RWTH-HPC/DRACC.git

### LLVM
* OpenMP tests directory in clang: https://github.com/llvm/llvm-project/tree/main/clang/test/OpenMP
* libomptarget tests in openmp: https://github.com/llvm/llvm-project/tree/main/openmp/libomptarget/test
