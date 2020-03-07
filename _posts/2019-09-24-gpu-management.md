# GPU resource management 
> Machine learning development using TensorFlow codebase (TF version 1.14.0) and one NVIDIA GPU (TESLA P100-PCIE-16GB)

Generally speaking, when using TensorFlow, Keras, or other TF-based frameworks, the management of GPU resources is automatically handled, regardless the number or the type of GPUs. But sometimes, we need to manually handle the GPU management and GPU memory allocation.

## what is the problem

When building machine learning framework in TensorFlow (or specifically Tensor2Tensor for machine translation), some uncommon errors of ```CUDA_ERROR_OUT_OF_MEMORY``` occurs on the GPU management. The output could be the following lines:
```
failed to allocate 15.10G from device: CUDA_OUT_OF_MEMORY: out of memory
........
failed to allocate 2.04G from device: CUDA_OUT_OF_MEMORY: out of memory
........
failed to allcoate 185.18M from device: CUDA_OUT_OF_MEMORY: out of memory
Check failed: err == cudaSuccess |$ err == cudaErrorInvalidValue Unexpected CUDA error: out of memory
Fatal Python error: Aborted
```

Oddly, it occurs when no python script is running and GPU is expected to be not used. However, when checking the GPU monitor ```nvidia-smi```, the output is as follows, meaning that no process is running but GPU memory is occupied somehow.
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.87.00    Driver Version: 418.87.00    CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla P100-PCIE...  On   | 00000000:04:00.0 Off |                    0 |
| N/A   53C    P0    25W / 250W |  15849MiB / 16280MiB |     87%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

Trying to reset the GPU and reallocate the resources would be the most intuitive choice. But running ```(sudo) nvidia-smi --gpu-reset -i 0``` would print the following results.
```
Unable to reset this GPU because it's being used by some other process (e.g. CUDA application, 
graphics application like X server, monitoring application like other instance of nvidia-smi). 
Please first kill all processes using this GPU and all compute applications running in the system 
(even when they are running on other GPUs) and then try to reset the GPU again.
Terminating early due to previous errors.
```

Clearly, the TensorFlow-based Python script is the key problem. Specifically, even though the tf-based Python script is shut down and no longer occupies CPU resources, GPU resources it occupied could not be somehow released. Moreover, commands like ```nvidia-smi``` cannot track the ```PID``` of the process that occupies the GPU resources or kill the session.

## how to address the problem

First of all, we need to list all the processes that occupy GPU resources (even agent process) using ```fuser``` commands.
```
sudo fuser -v /dev/nvidia*
```
which outputs the following lines
```
                    USER        PID     ACCESS      COMMAND
/dev/nvidia0:       root     kernel     mount       /dev/nvidia0
                    root      67193     F....       argusagent
                    admin    100562     F...m       t2t-decoder
/dev/nvidiactl:     root     kernel     mount       /dev/nvidiactl
                    root      67193     F....       argusagent
                    admin    100562     F...m       t2t-decoder
/dev/nvidia-uvm:    root     kernel     mount       /dev/nvidia-uvm
                    admin    100562     F...m       t2t-decoder
```
Here, the process that occupies GPU memory is found: PID-100562, the ```t2t-decoder``` process to decode source language to target language. The next step is to kill this process using ```kill -9 <PID>``` where ```<PID>``` is ```100562``` in this case. When checking ```nvidia-smi```, GPU memory usage decreases to ~25MiB, indicating that GPU resources are released, and problem is addressed.

This problem is honestly quite weird and uncommon. The most plausible reason is about the memory mechanism in TensorFlow (or frameworks built upon TF): the Python scripts not running fails to release GPU memory usage. Furthermore, the GPU is not used (25W/250W) but only the memory is occupied (15849MiB/16280MiB), so it could be the problem arising during building TF graphs or allocating TF variables to GPU devices. More work is expected to locate and debug these errors in TF codebase.

## other useful GPU-related commands 

```
nvidia-smi -i 0 -q
```
to list all available data on a particular GPU, with specified ID of the card with -i

```gpustat```, as a wrapper of ```nvidia-smi```, provides handy commands to get access to NVIDIA (only) GPUs. This command could be used as an alternative (a highly-recommended one) to ```nvidia-smi``` command to monitor the GPU devices.

![](images/gpustat.png)

```
$ gpustat
  -c: force colored output (or --color)
  -u: display username of the process owner (or --show-user)
  -c: display the process name (or --show-cmd)
  -f: display full command and cpu stats of running process (or --show-full-cmd)
  -p: display GPU power usage and/or limit (or --show-power)
```