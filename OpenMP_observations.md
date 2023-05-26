# OpenMP programming comments

## Pitfalls
* The syntax for mapping arrays requires at least one colon to indicate a range, otherwise it indicates an individual array element. 
The syntax is `[begin:end:step]`.   The `step` argument (and assocated colon) are optional, will be 1 by default.
The `begin` argument is optional, will be 0 by default.  The expressions `map(to: x[0:N])`, `map(to: x[:N])` and `map(to: x[0:N:1])` are the same.
* Begin/end iterators (pointers) are commonly used in C++.  The ending iterator (pointer) will not map correctly because it points to one past the end of the array.  The OpenMP runtime will not be able to map that location to a device pointer because the end pointer does not point to a mapped location in the host memory.  It is better to operate with a begin pointer and a length.
* Scalar values are firstprivate by default.  If the value is needed after the loop, the value needs to mapped back to the host.   This is different from shared memory OpenMP, where values outside the loop are shared by default, and the value after the loop can be assumed to be available.
