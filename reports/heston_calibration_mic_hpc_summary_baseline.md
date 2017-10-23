# Baseline Performance Summary - HestonCalibration

## Platform Information
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

## VTune Collection *hpc-performance*
```
    $ amplxe-cl -collect hpc-performance -- ./HestonCalibration.MIC
```
* Peak CPU Utilization: 14.1%
* Average CPU Usage: 38.344 out of 272 logical CPUs
    - These values are low and should be explored in detail
    - Perforance report also indicates that this code spends
      a significant portion of it's runtime in free/malloc
      calls, std::string operations and dynamic_cast<>. This
      may be an artifact of the short runtime.

* Parallel Region Time: 0.665 (25.4%)
* OpenMP Potential Gain: 0.502s (19.2%)
    - This code spends a significant amount of time in the OpenMP
      scheduler, which indicates non-ideal parallel work arrangement
      and should be explored in more detail.

* Back-End Bound: 82.3% of Pipeline Slots remain empty
    - This indicates that the code makes poor use of superscalar execution
    - Long latency operations e.g. divides and memory operations can cause this

* SIMD Instructions per Cycle: 0.0110
    * Packed SIMD Instr.: 75.7%
    * Scalar SIMD Instr.: 24.3%
    - A significant fraction of SIMD instructions are scalar and should
      explored in more detail

## Commentary
The primary observed outcome of profiling this model is the poor CPU
Utilization of 14.1% even though this code is explicitly making use of
OpenMP parallelization features of the Intel Compiler Collection. The
code also demonstrates poor backend utilization, with 82.3% of available
pipeline slots remaining empty. The code also demonstrates a poor mix
of Scalar to Packed SIMD instructions, and should be investigated for
improvements in code generation.

