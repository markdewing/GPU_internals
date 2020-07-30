# Programming models for GPUs

Collection of information about programming models.  Particularly about implementation and lower level information.
Knowing lower levels of abstraction is invaluable for debugging and reasoning about performance.

## OpenCL

* Khronos ICD loader: https://github.com/KhronosGroup/OpenCL-ICD-Loader

   Can be useful to build yourself to debug problems (for example, an OpenCL implementation library not showing up in the list of devices because it is silently failing at the dynamic link stage)
   
* OpenCL device simulator, Oclgrind: https://github.com/jrprice/Oclgrind
* Open source implementation, POCL: http://portablecl.org/

## OpenMP target offload

### LLVM/Clang
Guide to building LLVM/Clang with OpenMP offload: https://hpc-wiki.info/hpc/Building_LLVM/Clang_with_OpenMP_Offloading_to_NVIDIA_GPUs

The part about determing the GPU architecture is important: Clang/OpenMP must be built to support your hardware precisely.
The toolchain generates and uses `.cubin` SASS code (not PTX!), so that there is no backward/forward compability.

The failure mode is hard to diagnose, unfortunately.  The CUDA error is "device kernel image is invalid".

To get debugging information from the OpenMP library, configure LLVM with `-DLIBOMPTARGET_ENABLE_DEBUG=1` and set the enviroment variable `LIBOMPTARGET_DEBUG=1` at runtime.

## Kokkos
* Core library: https://github.com/kokkos/kokkos

## SYCL

## CUDA

## Example codes
My own repository for samples is here: https://github.com/markdewing/qmc_kernels

The `vector_add` kernel has the most implementations, being the simplest example that actually does something: https://github.com/markdewing/qmc_kernels/tree/master/kernels/vector_add

