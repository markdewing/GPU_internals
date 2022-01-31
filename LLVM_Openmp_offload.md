# OpenMP in LLVM

Home page: https://openmp.llvm.org/index.html

### Building

When building, determing the GPU architecture is important: Clang/OpenMP must be built to support your hardware precisely.
The toolchain generates and uses `.cubin` SASS code (not PTX!), so that there is no backward/forward compability.

The failure mode is hard to diagnose, unfortunately.  The CUDA error is "device kernel image is invalid".
Moving to the JIT model should ease this compatibility issue.

### Understanding the toolchain
The toolchain is much more complex for target offload.
* Use `-ccc-print-phases` to see all the toolchain steps.
* Use `-save-temps` to see all the intermediate files

### Runtime debugging

To get debugging information from the OpenMP library, configure LLVM with `-DLIBOMPTARGET_ENABLE_DEBUG=1` and set the enviroment variable `LIBOMPTARGET_DEBUG=1` at runtime.
