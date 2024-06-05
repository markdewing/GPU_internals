# Programming models for GPUs

Collection of information about programming models.  Particularly about implementation and lower level information.
Knowing lower levels of abstraction is invaluable for debugging and reasoning about performance.

## OpenCL

* Khronos ICD loader: https://github.com/KhronosGroup/OpenCL-ICD-Loader

   Can be useful to build yourself to debug problems (for example, an OpenCL implementation library not showing up in the list of devices because it is silently failing at the dynamic link stage)
   
* OpenCL device simulator, Oclgrind: https://github.com/jrprice/Oclgrind
* Open source implementation, POCL: http://portablecl.org/
* [SPIR-V](SPIRV.md) is used for an intermediate device-independent distribution format

## OpenMP target offload

### LLVM/Clang
See [LLVM OpenMP offload](LLVM_Openmp_offload.md)

### GCC
* https://gcc.gnu.org/wiki/Offloading

### Examples
See the [OpenMP_examples](OpenMP_examples.md) page.

### Observations
Observations and comments on programming OpenMP in the [OpenMP_observations](OpenMP_observations.md) page.


## Kokkos
* Core library: https://github.com/kokkos/kokkos

## SYCL

## CUDA

# AI/ML
Machine learning has given rise to a number of models for writing kernels
* [Triton language and compiler](https://triton-lang.org/main/index.html) and https://github.com/triton-lang/triton
  *   Not to be confused with the Triton Inference Server from NVidia
* Apache [TVM](https://tvm.apache.org/).  [TensorIR](https://tvm.apache.org/docs/tutorial/tensor_ir_blitz_course.html) is at the closest semantic level to Triton.  Nice tutorial from [Machine Learning Compliation](https://mlc.ai/chapter_introduction/index.html)

## Example codes
My own repository for samples is here: https://github.com/markdewing/qmc_kernels

The `vector_add` kernel has the most implementations, being the simplest example that actually does something: https://github.com/markdewing/qmc_kernels/tree/master/kernels/vector_add

