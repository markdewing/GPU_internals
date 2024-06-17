# Triton
* Homepage: https://triton-lang.org/main/index.html
* Github: https://github.com/triton-lang/triton

## Internals
At the coarest level, Triton compiles code and launches it on the GPU.

The key function appears to be the `run` function on the `JITFunction` class 
https://github.com/triton-lang/triton/blob/23295314ec695711e9c3ae3fd39f5ba488f0b9ab/python/triton/runtime/jit.py#L587

It checks a cache (associated with this object) and compiles the kernel if not found.
Compilation is called here: https://github.com/triton-lang/triton/blob/23295314ec695711e9c3ae3fd39f5ba488f0b9ab/python/triton/runtime/jit.py#L644

Next, the kernel is run on the GPU
https://github.com/triton-lang/triton/blob/23295314ec695711e9c3ae3fd39f5ba488f0b9ab/python/triton/runtime/jit.py#L673


### Compilation
The `compile` function in compiler/compiler.py 
https://github.com/triton-lang/triton/blob/23295314ec695711e9c3ae3fd39f5ba488f0b9ab/python/triton/compiler/compiler.py#L226

This checks the cache (defaults to the file cache located at `~/.triton/cache`)

The cache directory contains the kernels - metadata files, various IR stages, PTX, and the .cubin file (which is what gets loaded and run)

### Kernel Launch
The `CompiledKernel` `__init__` function reads the data from the files, including the compiled binary.
https://github.com/triton-lang/triton/blob/23295314ec695711e9c3ae3fd39f5ba488f0b9ab/python/triton/compiler/compiler.py#L336

For NVidia, the call to extract the kernel from the module data occurs in `loadBinary` in `third_party/nvidia/backend/driver.c` (calls `cuModuleLoadData`) https://github.com/triton-lang/triton/blob/cd4a172c79c69fff91b893c2e5deb78a7a887c26/third_party/nvidia/backend/driver.c#L82 
It's exposed to Python as `load_binary` in `CudaUtils`.
