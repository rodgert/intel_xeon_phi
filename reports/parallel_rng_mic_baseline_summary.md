#Baseline Performance Summary - ParallelRNG

##Platform Information
    Operating System
    ```
        3.10.0-693.el7.x86_64
        NAME="CentOS Linux" VERSION="7 (Core)"
        ID="centos" ID_LIKE="rhel fedora" VERSION_ID="7"
        PRETTY_NAME="CentOS Linux 7 (Core)"
    ```

    CPU
    ```
        Name: Intel(R) Processor code named Knights Landing
        Frequency: 1.397 GHz
        Logical CPU Count: 272
    ```

##VTune Collection *hpc-performance*
```
    $ amplxe-cl -collect hpc-performance -- ./ParallelRNG.MIC
```
    * Peak CPU Utilization: 18.9%
    * Average CPU Usage: 51.354 out of 272 logical CPUs
        - These values are low and should be explored in detail

    * Back-End Bound: 81.9% of Pipeline Slots remain empty
        - This indicates that the code makes poor use of superscalar execution
        - Long latency operations e.g. divides and memory operations can cause this

    * L2 Hit Bound: 0.4% of clock ticks
    * L2 Miss bound: 0.0% of clock ticks
        * Demand Misses: 0.0% of L2 Input Requests
        * HW Prefetcher: 100.0% of L2 Input Requests
    * MCDRAM Bandwidth Bound: 0.0%
    * DRAM Bandwidth Bound: 1.2%

    * SIMD Instructions per Cycle: 0.009
        * Packed SIMD Instr.: 100.0%
        * Scalar SIMD Instr.: 0.0%

##Commentary
    The primary observed outcome of profiling this model is the poor CPU
    Utilization of 18.9% even though this code is explicitly making use of
    OpenMP parallelization features of the Intel Compiler Collection. The
    code also demonstrates poor backend utilization, with 81.9% of available
    pipeline slots remaining empty. This may, however, be a product of the
    type of random number generator in use, relying on instructions which
    are bound to a single execution port within the ALU.
