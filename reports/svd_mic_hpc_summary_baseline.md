#Baseline Performance Summary - SVD

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
    $ amplxe-cl -collect hpc-performance -- ./SVD.MIC
```
    * Peak CPU Utilization: 2.4%
    * Average CPU Usage: 6.437 out of 272 logical CPUs
        - These values are low and should be explored in detail

## Commentary
    The primary observed outcome of profiling this model is the poor CPU
    Utilization of 2.4% even though this code is explicitly making use of
    OpenMP parallelization features of the Intel Compiler Collection. The
    code does not appear to run long enough, or scale enough to generatei
    any significant memory bandwidth, so all memory related counts report
    as 0.0%
