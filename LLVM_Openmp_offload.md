# OpenMP in LLVM

Home page: https://openmp.llvm.org/index.html

Design of offload: https://clang.llvm.org/docs/OffloadingDesign.html

### Building

When building, determing the GPU architecture is important: Clang/OpenMP must be built to support your hardware precisely.
The toolchain generates and uses `.cubin` SASS code (not PTX!), so that there is no backward/forward compability.

The failure mode is hard to diagnose, unfortunately.  The CUDA error is "device kernel image is invalid".
Moving to the JIT model should ease this compatibility issue.

### Understanding the toolchain
The toolchain is much more complex for target offload.
* Use `-ccc-print-phases` to see all the toolchain steps and dependency hierarchy.
* Use `-ccc-print-bindings` to see bindings of tools to actions (outputs the triple, the tool, input files, and output files).
* Use `-save-temps` to see all the intermediate files (this also adds extra steps with human-readable output, like the preprocessor and the assembler)
* Use `-v` (`-verbose`) to see all the tool invocations command lines.

For a host-only program the compiler has a couple of sub-steps: the compilation of code to an object file, and the linking of the object file(s).
For target offload, there is also compilation to object code for the offload device and combining different objects into the final executable (beyond what a normal linker does)

The [clang-offload-packager](https://clang.llvm.org/docs/ClangOffloadPackager.html) tool embeds device code into the host file. (The offload packager replaced the offload wrapper tool).
The [clang-linker-wrapper](https://clang.llvm.org/docs/ClangLinkerWrapper.html) will pull out device code (in `.llm.offloading`) and call the regular linker.

The embedded device code can be extracted with `clang-offload-packager file.o --image=file=out.bc`.

### Targets
How to know what value to use for `-fopenmp-targets`?

Most likely you will want:

* amdgcn-amd-amdhsa
* nvptx64-nvidia-cuda
* x86_64-pc-linux-gnu

Shortened versions, `amdgcn` and `nvptx64` also work.
  

There most definitive list is in https://github.com/llvm/llvm-project/blob/main/openmp/libomptarget/CMakeLists.txt

### Runtime debugging

To get debugging information from the OpenMP library, configure LLVM with `-DLIBOMPTARGET_ENABLE_DEBUG=1` and set the enviroment variable `LIBOMPTARGET_DEBUG=1` at runtime.
