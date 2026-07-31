Parallel Data Aggregation Project

This project compares different methods of data aggregation in C++.

The project contains:
- Single-thread execution
- OpenMP parallel execution
- MPI parallel execution

main.cpp contains the single-thread and OpenMP code.

mpi_sum.cpp contains the MPI implementation.

The screenshots folder contains:
- program outputs
- execution screenshots
- performance graph

Commands Used:

Compile main.cpp:
g++ main.cpp -fopenmp -o main

Run:
./main

Compile MPI code:
g++ mpi_sum.cpp -I"C:\Program Files (x86)\Microsoft SDKs\MPI\Include" -L"C:\Program Files (x86)\Microsoft SDKs\MPI\Lib\x64" -lmsmpi -o mpi_sum

Note:
The MPI compile command may vary depending on the installation path of Microsoft MPI on the system.

Run MPI:
mpiexec -n 4 mpi_sum