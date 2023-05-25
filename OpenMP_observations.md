# OpenMP programming comments

## Pitfalls
* The syntax for mapping arrays requires at least one colon to indicate a range, otherwise it indicates an individual array element. 
The syntax is `[begin:end:step]`.   The `step` argument (and assocated colon) are optional, will be 1 by default.
The `begin` argument is optional, will be 0 by default.  The expressions `map(to: x[0:N])`, `map(to: x[:N])` and `map(to: x[0:N:1])` are the same.
