# GPU Internals
Documentation on how GPU's work internally and communicate externally.  Mostly focused on compute applications.

By Vendor
## NVIDIA
* https://nvidia.github.io/open-gpu-doc/
* https://github.com/envytools/envytools

### Assembly
* [Nvidia assembly](Nvidia_assembly.md)

### Projects that might yield interesting info
* https://github.com/pakmarkthub/dragon direct resource access for GPUs over NVM (similer to mmap on CPUs)
* https://github.com/yalue/cuda_scheduling_examiner_mirror
* https://github.com/NVlabs/NVBit Binary instrumentation tool

## AMD
* https://rocmdocs.amd.com/en/latest/  ROCm documentation
### Assembly
* https://developer.amd.com/resources/developer-guides-manuals/

## Intel
* https://software.intel.com/content/www/us/en/develop/articles/introduction-to-gen-assembly.html

## Raspberry Pi
* https://github.com/mn416/QPULib library for programming QPU (Quad Processing Units)
* https://github.com/doe300/VC4CL  OpenCL implementation


# Questions

* How do transfers across the bus (usually PCI-e) work?
  * NVidia has GPUDirect which can transfer from device to device (and bypass the CPU)  https://docs.nvidia.com/cuda/gpudirect-rdma/index.html
  * Does this change with other buses (CAPI, NVLink?)
  
* How does memory management work?
   * How is the memory map maintained?  There must be some sort of MMU to provide memory protections.  How does it work?

* What is the lifecycle of a kernel in detail?
  * It must be something like
    1. Copy kernel to GPU memory
    2. Start executing kernel (how?)
    3. Signal that the kernel is finished
    
 * Examples on visualizing the execution behavior of kernels
