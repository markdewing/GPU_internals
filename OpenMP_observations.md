# OpenMP programming comments

## Pitfalls
* The syntax for mapping arrays requires at least one colon to indicate a range, otherwise it indicates an individual array element. 
The syntax is `[begin:end:step]`.   The `step` argument (and assocated colon) are optional, will be 1 by default.
The `begin` argument is optional, will be 0 by default.  The expressions `map(to: x[0:N])`, `map(to: x[:N])` and `map(to: x[0:N:1])` are the same.
* Begin/end iterators (pointers) are commonly used in C++.  The ending iterator (pointer) will not map correctly because it points to one past the end of the array.  The OpenMP runtime will not be able to map that location to a device pointer because the end pointer does not point to a mapped location in the host memory.  It is better to operate with a begin pointer and a length.
* Scalar values are firstprivate by default.  If the value is needed after the loop, the value needs to mapped back to the host.   This is different from shared memory OpenMP, where values outside the loop are shared by default, and the value after the loop can be assumed to be available.


## Questions
### How to ensure code is actually running on the device?
1. The environment variable `LIBOMPTARGET_INFO` will produce information about data mapping, data transfers, and kernel launches.  Set to `-1` for all information.  See [LLVM OpenMP documentation on LIBOMPTARGET_INFO](https://openmp.llvm.org/design/Runtimes.html#libomptarget-info) for more details.  This variable only works for the LLVM OpenMP library and libraries derived from it.
2. The runtime function [`omp_is_initial_device`](https://www.openmp.org/spec-html/5.1/openmpsu166.html#x210-2430003.7.6) can be used to determined if an offload region is running on the host or device.  The following code can be used to test the compiler and installation.
```
  int isHost = -1;

#pragma omp target map(from : isHost)
  { isHost = omp_is_initial_device(); }

  if (isHost < 0) {
    printf("Target region not executed, isHost = %d\n", isHost);
  } else if (isHost == 0) {
    printf("Offload okay, isHost = %d\n", isHost);
  } else {
    printf("Not offloaded, running on host, isHost = %d\n", isHost);
  }
```
3. The LLVM compiler has the flag `-fopenmp-offload-mandatory`.  When specified, it skips creating a host fallback at compile-time.  However, if there is no device present at runtime, it will simply not execute anything (I would expect an error message).  Setting the environment variable [`OMP_TARGET_OFFLOAD`](https://www.openmp.org/spec-html/5.1/openmpse74.html#x340-5150006.17) to `mandatory` does result in a runtime error message, as expected.
