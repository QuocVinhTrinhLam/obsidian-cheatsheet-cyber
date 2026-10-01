#### CPU (Central Processing Unit)

- `Core Count`: Typically ranges from 2 to 16 cores in consumer-grade systems.
- `Design Purpose`: Optimized for sequential execution. Ideal for tasks that require handling instructions one at a time.
- `Parallelism`: While modern CPUs support some level of parallel processing via multithreading, they are not inherently designed for massive parallelization.
- `Use in Cracking`: CPU-based cracking is practical when GPU resources aren't available. It's fully capable of performing the task, but is significantly slower in comparison.
- `Best For`: Smaller hash sets, testing scripts, or environments with limited hardware access.

#### GPU (Graphics Processing Unit)

- `Core Count`: Typically equipped with hundreds to thousands of smaller cores built specifically for parallel workloads.
- `Design Purpose`: Built for high-throughput parallel processing, allowing simultaneous computation of many operations. Perfect for tasks like hash generation.
- `Use in Cracking`: Ideal for brute-force, dictionary, or mask-based attacks, where millions of hash computations per second are needed.
- `Best For`: Large-scale password cracking efforts where performance and speed are critical.
## Hash Rate & Volume: CPU vs. GPU

![](CPU%20vs%20GPU%20Based%20Cracking-20260923-084713.png)
#### Example Scenario

To illustrate the difference, let's imagine a simplified scenario: each core, whether on a CPU or GPU, can compute one hash per second. While this isn't realistic, it helps us understand the concept.

- `CPU with 4 cores` = 4 hashes/second → 4 password attempts/second
- `GPU with 16 cores` = 16 hashes/second → 16 password attempts/second

Even with identical core performance, the greater number of cores on the GPU allows it to achieve a much higher hash rate.
## Identifying Available Devices with Hashcat

```sh
3kjS@htb[/htb]$ hashcat -I
```
## Selecting Device Type and ID

Hashcat supports multiple [OpenCL](https://developer.nvidia.com/opencl) device types, allowing us to choose what kind of hardware to leverage for cracking:

1. `CPU`: Useful for hashes that require complex logic or when GPU resources are unavailable.
2. `GPU`: Ideal for high-speed cracking due to their ability to handle massive parallel computations.
3. `FPGA, DSP, or Co-Processor`: Specialized hardware accelerators that can be used in specific environments for enhanced performance.

To specify which OpenCL device to use in Hashcat, the `-D` flag is used to define the type of device:

- `-D 1` tells Hashcat to use the CPU
- `-D 2` tells Hashcat to use the GPU

In addition to this, the `-d` flag allows you to select specific device IDs within the chosen category:

- `-d 1` selects Backend Device ID #1
- `-d 5` selects Backend Device ID #5

```sh
3kjS@htb[/htb]$ hashcat -m 22000 -D 2 -d 8 hash /opt/wordlist.txt 

hashcat (v6.2.5) starting

You have enabled --force to bypass dangerous warnings and errors!
This can hide serious problems and should only be done when debugging.
Do not report hashcat issues encountered when using --force.

OpenCL API (OpenCL 1.2 CUDA) - Platform #3 [The pocl project]
=============================================================
* Device #8: OpenCL 1.2 CUDA, NVIDIA Corporation
```
## Understanding and Defining Hashcat Workloads

The Low workload level (`-w 1`) uses minimal resources and is suitable for systems with limited power. The Default level (`-w 2`) offers a balanced approach between performance and resource usage. The High level (`-w 3`) increases computation and power usage for faster results. Lastly, the Nightmare level (`-w 4`) pushes the system to its maximum capacity, delivering the highest speed at the cost of maximum power consumption. These options allow users to fine-tune their cracking performance based on available resources.

Hashcat offers four distinct workload profiles for GPU-based attacks:

- `Low`: Minimal power consumption and computational load, suitable for multitasking or running in environments with limited resources.
- `Default`: Balanced power consumption and computational effort, providing a compromise between speed and resource usage.
- `High`: Increased power consumption and computational intensity, designed for faster cracking at the cost of higher resource utilization.
- `Nightmare`: Maximum power consumption and computational load, prioritizing cracking speed over all other considerations.

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt -w 3
```
