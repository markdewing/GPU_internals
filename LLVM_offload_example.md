## Example with tool chain details

Command line
```
clang++ -O2 -fopenmp \
-save-temps \
-fopenmp-targets=amdgcn-amd-amdhsa \
vector_add.cpp
```


Output from `-ccc-print-bindings`
```
# "x86_64-unknown-linux-gnu" - "clang", inputs: ["vector_add.cpp"], output: "vector_add-host-x86_64-unknown-linux-gnu.ii"
# "x86_64-unknown-linux-gnu" - "clang", inputs: ["vector_add-host-x86_64-unknown-linux-gnu.ii"], output: "vector_add-host-x86_64-unknown-linux-gnu.bc"
# "x86_64-unknown-linux-gnu" - "clang", inputs: ["vector_add-host-x86_64-unknown-linux-gnu.bc"], output: "vector_add-host-x86_64-unknown-linux-gnu.s"
# "x86_64-unknown-linux-gnu" - "clang::as", inputs: ["vector_add-host-x86_64-unknown-linux-gnu.s"], output: "vector_add-host-x86_64-unknown-linux-gnu.o"
# "amdgcn-amd-amdhsa" - "clang", inputs: ["vector_add.cpp"], output: "vector_add-openmp-amdgcn-amd-amdhsa.ii"
# "amdgcn-amd-amdhsa" - "clang", inputs: ["vector_add-openmp-amdgcn-amd-amdhsa.ii", "vector_add-host-x86_64-unknown-linux-gnu.bc"], output: "vector_add-openmp-amdgcn-amd-amdhsa.bc"
# "amdgcn-amd-amdhsa" - "AMDGCN::OpenMPLinker", inputs: ["vector_add-openmp-amdgcn-amd-amdhsa.bc"], output: "a.out-openmp-amdgcn-amd-amdhsa"
# "x86_64-unknown-linux-gnu" - "offload wrapper", inputs: ["a.out-openmp-amdgcn-amd-amdhsa"], output: "a-host-x86_64-unknown-linux-gnu.bc"
# "x86_64-unknown-linux-gnu" - "clang", inputs: ["a-host-x86_64-unknown-linux-gnu.bc"], output: "a-host-x86_64-unknown-linux-gnu.s"
# "x86_64-unknown-linux-gnu" - "clang::as", inputs: ["a-host-x86_64-unknown-linux-gnu.s"], output: "a-host-x86_64-unknown-linux-gnu.o"
# "x86_64-unknown-linux-gnu" - "GNU::Linker", inputs: ["vector_add-host-x86_64-unknown-linux-gnu.o", "a-host-x86_64-unknown-linux-gnu.o"], output: "a.out"
```

Output from `-ccc-print-phases`

```
            +- 0: input, "vector_add.cpp", c++, (host-openmp)
         +- 1: preprocessor, {0}, c++-cpp-output, (host-openmp)
      +- 2: compiler, {1}, ir, (host-openmp)
   +- 3: backend, {2}, assembler, (host-openmp)
+- 4: assembler, {3}, object, (host-openmp)
|                       |     +- 5: input, "vector_add.cpp", c++, (device-openmp)
|                       |  +- 6: preprocessor, {5}, c++-cpp-output, (device-openmp)
|                       |- 7: compiler, {6}, ir, (device-openmp)
|                    +- 8: offload, "host-openmp (x86_64-unknown-linux-gnu)" {2}, "device-openmp (amdgcn-amd-amdhsa)" {7}, ir
|                 +- 9: backend, {8}, assembler, (device-openmp)
|              +- 10: assembler, {9}, object, (device-openmp)
|           +- 11: linker, {10}, image, (device-openmp)
|        +- 12: offload, "device-openmp (amdgcn-amd-amdhsa)" {11}, image
|     +- 13: clang-offload-wrapper, {12}, ir, (host-openmp)
|  +- 14: backend, {13}, assembler, (host-openmp)
|- 15: assembler, {14}, object, (host-openmp)
16: linker, {4, 15}, image, (host-openmp)
```


### Verbose output
Verbose output with collapsible command line options and labeled with the phases.

<details>
 <summary>1: preprocessor, {0}, c++-cpp-output, (host-openmp) </summary>
<ul style="list-style: none;">

<li><details>
   <summary> Tool with inputs and outputs
   <pre>
/mnt/nvme/software/llvm/git/usr/bin/clang-15 \
vector_add.cpp \
-o vector_add-host-x86_64-unknown-linux-gnu.ii \
   </pre>
   </summary>
   More command line options
 
 ```
-cc1 \
-triple \
x86_64-unknown-linux-gnu \
-E \
-save-temps=cwd \
-disable-free \
-clear-ast-before-backend \
-main-file-name vector_add.cpp \
-mrelocation-model static \
-mframe-pointer=none \
-fmath-errno \
-ffp-contract=on \
-fno-rounding-math \
-mconstructor-aliases \
-funwind-tables=2 \
-target-cpu x86-64 \
-tune-cpu generic \
-debugger-tuning=gdb \
-v \
-fcoverage-compilation-dir=/home/mdewing/nvme/physics/codes/qmcpack/qmc_kernels/kernels/vector_add/openmp \
-resource-dir /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0 \
-I/opt/intel/compilers_and_libraries_2016.1.150/linux/mkl/include \
-internal-isystem  /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/c++/9 \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/x86_64-linux-gnu/c++/9 \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/c++/9/backward \
-internal-isystem /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0/include \
-internal-isystem /usr/local/include \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../x86_64-linux-gnu/include \
-internal-externc-isystem /usr/include/x86_64-linux-gnu \
-internal-externc-isystem /include \
-internal-externc-isystem /usr/include \
-internal-isystem /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0/include \
-internal-isystem /usr/local/include \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../x86_64-linux-gnu/include \
-internal-externc-isystem /usr/include/x86_64-linux-gnu \
-internal-externc-isystem /include \
-internal-externc-isystem /usr/include \
-O2 \
-fdeprecated-macro \
-fdebug-compilation-dir=/home/mdewing/nvme/physics/codes/qmcpack/qmc_kernels/kernels/vector_add/openmp \
-ferror-limit 19 \
-fopenmp \
-fgnuc-version=4.2.1 \
-fcxx-exceptions \
-fexceptions \
-vectorize-loops \
-vectorize-slp \
-fopenmp-targets=amdgcn-amd-amdhsa \
-faddrsig \
-D__GCC_HAVE_DWARF2_CFI_ASM=1 \
-x c++ 
```
 </details></li>
 </ul>
 </details>
 
 (skip some of the host steps)
 
 <details>
 <summary>6: preprocessor, {5}, c++-cpp-output, (device-openmp) </summary>
<ul style="list-style: none;">

<li><details>
   <summary> Tool with inputs and outputs
   <pre>
 /mnt/nvme/software/llvm/git/usr/bin/clang-15 \
vector_add.cpp \
-o  vector_add-openmp-amdgcn-amd-amdhsa.ii \
</pre>
  </summary>

More command line options
```
-cc1 \
-triple  amdgcn-amd-amdhsa \
-aux-triple  x86_64-unknown-linux-gnu \
-E \
-save-temps=cwd \
-disable-free \
-clear-ast-before-backend \
-main-file-name  vector_add.cpp \
-mrelocation-model pic \
-pic-level 2 \
-fhalf-no-semantic-interposition \
-mframe-pointer=none \
-ffp-contract=on \
-fno-rounding-math \
-mconstructor-aliases \
-target-cpu gfx900 \
-fcuda-is-device \
-mlink-builtin-bitcode \
/mnt/nvme/software/llvm/git/usr/lib/libomptarget-amdgpu-gfx900.bc \
-debugger-tuning=gdb \
-v \
-resource-dir /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0 \
-internal-isystem /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0/include/openmp_wrappers \
-include  __clang_openmp_device_functions.h \
-I/opt/intel/compilers_and_libraries_2016.1.150/linux/mkl/include \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/c++/9 \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/x86_64-linux-gnu/c++/9 \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../include/c++/9/backward \
-internal-isystem /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0/include \
-internal-isystem /usr/local/include \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../x86_64-linux-gnu/include \
-internal-externc-isystem /usr/include/x86_64-linux-gnu \
-internal-externc-isystem /include \
-internal-externc-isystem /usr/include \
-internal-isystem /mnt/nvme/software/llvm/git/usr/lib/clang/15.0.0/include \
-internal-isystem /usr/local/include \
-internal-isystem /usr/lib/gcc/x86_64-linux-gnu/9/../../../../x86_64-linux-gnu/include \
-internal-externc-isystem /usr/include/x86_64-linux-gnu \
-internal-externc-isystem /include \
-internal-externc-isystem /usr/include \
-O2 \
-fdeprecated-macro \
-fdebug-compilation-dir=/home/mdewing/nvme/physics/codes/qmcpack/qmc_kernels/kernels/vector_add/openmp \
-ferror-limit 19 \
-fvisibility protected \
-fopenmp \
-fgnuc-version=4.2.1 \
-fcxx-exceptions \
-fexceptions \
-vectorize-loops \
-vectorize-slp \
-fopenmp-is-device \
-faddrsig \
-D__GCC_HAVE_DWARF2_CFI_ASM=1 \
-x c++ 
```
</li></details>
