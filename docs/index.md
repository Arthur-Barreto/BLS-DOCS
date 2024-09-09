# Homepage

This work addresses the parallel implementation of the Box Least Squares (BLS) algorithm for planetary transit fitting. This algorithm is crucial for analyzing data from satellite missions, such as the KEPLER and K2 missions discussed in this project. The study focuses on optimizing the algorithm using parallelism frameworks, including OpenMP, MPI, GPUs via PyTorch, and STAPL, a library that abstracts the use of OpenMP and MPI. As a suggested improvement for this project, the optimization of the candidate generation algorithm for BLS is highlighted, as the transit period and other parameters must be correctly included in a comprehensive list of candidates to be tested.

## Documentation

For more information on the proejct, please refer to the following documentation:

- [Sequential](sequential.md)
- [OpenMP](omp.md)
- [MPI](mpi.md)
- [STAPL](stapl.md)
- [PyTorch](pytorch.md)

Each of these documentation files provides a comprehensive explanation of the implementation, including detailed documentation for each function.

## Results

put here a brief summary of the results

## How to contribute

If you would like to contribute to this project, please refer to the [contributing](contributing.md) page for more information.
