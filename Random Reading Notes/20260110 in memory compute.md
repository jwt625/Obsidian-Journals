
2026-01-10T09:20:04-08:00

Soundtrack: Michiko & Hatchin OST

### [1.1 computing's energy problem (and what we can do about it)](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/6757323/%3Fcasa_token%3D7XSsl0DT8YEAAAAA:Oy2ULWq5exPY31WrihCVHYEBmxmMx3RxRsaPsHBoIxxtpxdf5TlO1jcn57n0hFVFdOkzKcDQ&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=2649055063900027678&ei=VYpiaaKBAdKV6rQP84i9kQc&authuser=1&scisig=AHkA5jRitDjkjDpINMmE39aPxOxC)

M Horowitz - 2014 IEEE international solid-state circuits conference …, 2014

Our challenge is clear: The drive for performance and the end of voltage scaling have made power, and not the number of transistors, the principal factor limiting further improvements in computing performance. Continuing to scale compute performance will require the creation and effective use of new specialized compute engines, and will require the participation of application experts to be successful. If we play our cards right, and develop the tools that allow our customers to become part of the design process, we will create a new wave of innovative and efficient computing devices.


* **AMAE**: Average Memory Access Energy
* **ASIC**: Application-Specific Integrated Circuit
* **CMOS**: Complementary Metal-Oxide-Semiconductor
* **CPU**: Central Processing Unit
* **DARPA**: Defense Advanced Research Projects Agency
* **DRAM**: Dynamic Random Access Memory
* **DSP**: Digital Signal Processor
* **DSL**: Domain Specific Language
* **DVFS**: Dynamic Voltage and Frequency Scaling
* **finFET**: Fin Field-Effect Transistor
* **FO4**: Fan-Out of 4 (delay of an inverter driving a load 4x its input capacitance)
* **FP**: Floating-Point
* **GPU**: Graphics Processing Unit
* **I/O**: Input/Output
* **IC**: Integrated Circuit
* **IEEE**: Institute of Electrical and Electronics Engineers
* **ISSCC**: International Solid-State Circuits Conference
* **MOPS**: Million Operations Per Second
* **nMOS**: n-channel Metal-Oxide Semiconductor
* **SIMD**: Single Instruction, Multiple Data
* **SoC**: System on Chip
* **SPEC**: Standard Performance Evaluation Corporation (Computing Benchmark)
* **VLSI**: Very-Large-Scale Integration


http://www.spiral.net/  
- Franchetti2019: [SPIRAL: AI for High Performance Code]([http://spiral.net/publications/franchetti-ornl2019.pdf](https://t.co/igfvR3yU1Q))
![[Pasted image 20260110095140.png]]


### [Architectural Modeling and Benchmarking for Digital DRAM PIM](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/10763591/%3Fcasa_token%3D_ZGhU9C1bucAAAAA:yakzU9gxjRrBt7vSihll5tZdWIClPITeMwZyO0qa_YVmgeMxOxtDpR9GwnwD0iOYoBGDDjw7&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=2600521805144235720&ei=_pxiaf_HF6C16rQPm4fPgQQ&authuser=1&scisig=AHkA5jR_Jl7OrdAH8c7NfKllfiUc)

FA Siddique, D Guo, Z Fan, M Gholamrezaei… - 2024 IEEE International …, 2024
- https://www.cs.virginia.edu/venkat/papers/iiswc2024.pdf

Processing In Memory (PIM) integrates computational logic units directly into the memory architecture, offering significant performance improvements for memory-bound applications such as matrix operations, vector operations, and database applications compared to general-purpose CPUs or GPUs. However, the lack of a standardized benchmark suite and simulation framework poses a challenge in exploring, evaluating, and designing different PIM architectures. This paper addresses this gap by introducing a comprehensive benchmark suite, PIMbench, along with a performance and energy modeling framework, PIMeval, both designed to support a wide range of PIM architectures for DRAM. This paper also proposes a set of PIM APIs for writing PIM programs, enabling benchmarks to be executed across different PIM architectures, and allowing for comparing the performance of bit-serial and bit-parallel subarray-level PIM and bank-level PIM. PIMbench and PIMeval have been open-sourced and can be accessed at: https://github.com/UVA-LavaLab/PIMeval-PIMbench




### [Computedram: In-memory compute using off-the-shelf drams](https://scholar.google.com/scholar_url?url=https://dl.acm.org/doi/abs/10.1145/3352460.3358260&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=6121214841476829011&ei=gJFiac7MFLK16rQPn86DwQ8&authuser=1&scisig=AHkA5jQhoTJJMfbbMhQiEesJYUid)

F Gao, G Tziantzioulis, D Wentzlaff - Proceedings of the 52nd annual IEEE/ACM …, 2019

In-memory computing has long been promised as a solution to the "Memory Wall" problem. Recent work has proposed using chargesharing on the bit-lines of a memory in order to compute in-place and with massive parallelism, all without having to move data across the memory bus. Unfortunately, prior work has required modification to RAM designs (e.g. adding multiple row decoders) in order to open multiple rows simultaneously. So far, the competitive and low-margin nature of the DRAM industry has made commercial DRAM manufacturers resist adding any additional logic into DRAM. This paper addresses the need for in-memory computation with little to no change to DRAM designs. It is the first work to demonstrate in-memory computation with off-the-shelf, unmodified, commercial, DRAM. This is accomplished by violating the nominal timing specification and activating multiple rows in rapid succession, which happens to leave multiple rows open simultaneously, thereby enabling bit-line charge sharing. We use a constraint-violating command sequence to implement and demonstrate row copy, logical OR, and logical AND in unmodified, commodity, DRAM. Subsequently, we employ these primitives to develop an architecture for arbitrary, massively-parallel, computation. Utilizing a customized DRAM controller in an FPGA and commodity DRAM modules, we characterize this opportunity in hardware for all major DRAM vendors. This work stands as a proof of concept that in-memory computation is possible with unmodified DRAM modules and that there exists a financially feasible way for DRAM manufacturers to support in-memory compute.

![[Pasted image 20260110095122.png]]

![[Pasted image 20260110104435.png]]



### [Benchmarking a new paradigm: Experimental analysis and characterization of a real processing-in-memory system](https://ieeexplore.ieee.org/abstract/document/9771457/)

[J Gómez-Luna](https://scholar.google.com/citations?user=UXlFLY0AAAAJ&hl=en&oi=sra), [I El Hajj](https://scholar.google.com/citations?user=_VVw504AAAAJ&hl=en&oi=sra), [I Fernandez](https://scholar.google.com/citations?user=94I-gKEAAAAJ&hl=en&oi=sra)… - IEEE …, 2022 - ieeexplore.ieee.org

Many modern workloads, such as neural networks, databases, and graph processing, are fundamentally memory-bound. For such workloads, the data movement between main memory and CPU cores imposes a significant overhead in terms of both latency and energy. A major reason is that this communication happens through a narrow bus with high latency and limited bandwidth, and the low data reuse in memory-bound workloads is insufficient to amortize the cost of main memory access. Fundamentally addressing this data movement bottleneck requires a paradigm where the memory system assumes an active role in computing by integrating processing capabilities. This paradigm is known as processing-in-memory (PIM). Recent research explores different forms of PIM architectures, motivated by the emergence of new 3D-stacked memory technologies that integrate memory with a logic layer where processing elements can be easily placed. Past works evaluate these architectures in simulation or, at best, with simplified hardware prototypes. In contrast, the UPMEM company has designed and manufactured the first publicly-available real-world PIM architecture. The UPMEM PIM architecture combines traditional DRAM memory arrays with general-purpose in- order cores, called DRAM Processing Units (DPUs), integrated in the same chip. This paper provides the first comprehensive analysis of the first publicly-available real-world PIM architecture. We make two key contributions. First, we conduct an experimental characterization of the UPMEM-based PIM system using microbenchmarks to assess various architecture limits such as compute throughput and memory bandwidth, yielding new insights. Second, we present PrIM (Processing-In-Memory benchmarks), a benchmark suite of 16 workloads from different application domains (e.g., dense/sparse linear algebra, databases, data analytics, graph processing, neural networks, bioinformatics, image processing), which we identify as memory-bound. We evaluate the performance and scaling characteristics of PrIM benchmarks on the UPMEM PIM architecture, and compare their performance and energy consumption to their modern CPU and GPU counterparts. Our extensive evaluation conducted on two real UPMEM-based PIM systems with 640 and 2,556 DPUs provides new insights about suitability of different workloads to the PIM system, programming recommendations for software designers, and suggestions and hints for hardware and architecture designers of future PIM systems.
![[Pasted image 20260110111610.png]]
![[Pasted image 20260110111623.png]]




### [Neupims: Npu-pim heterogeneous acceleration for batched llm inferencing](https://dl.acm.org/doi/abs/10.1145/3620666.3651380)

[G Heo](https://scholar.google.com/citations?user=Ay9frzEAAAAJ&hl=en&oi=sra), [S Lee](https://scholar.google.com/citations?user=lv6Qvf8AAAAJ&hl=en&oi=sra), [J Cho](https://scholar.google.com/citations?user=55_oAlgAAAAJ&hl=en&oi=sra), [H Choi](https://scholar.google.com/citations?user=OxN0LfUAAAAJ&hl=en&oi=sra), [S Lee](https://scholar.google.com/citations?user=hs2Y4RYAAAAJ&hl=en&oi=sra), [H Ham](https://scholar.google.com/citations?user=f21SJrkAAAAJ&hl=en&oi=sra)… - Proceedings of the 29th …, 2024 - dl.acm.org

Modern transformer-based Large Language Models (LLMs) are constructed with a series of decoder blocks. Each block comprises three key components: (1) QKV generation, (2) multi-head attention, and (3) feed-forward networks. In batched processing, QKV generation and feed-forward networks involve compute-intensive matrix-matrix multiplications (GEMM), while multi-head attention requires bandwidth-heavy matrix-vector multiplications (GEMV). Machine learning accelerators like TPUs or NPUs are proficient in handling GEMM but are less efficient for GEMV computations. Conversely, Processing-in-Memory (PIM) technology is tailored for efficient GEMV computation, while it lacks the computational power to handle GEMM effectively.

Inspired by this insight, we propose NeuPIMs, a heterogeneous acceleration system that jointly exploits a conventional GEMM-focused NPU and GEMV-optimized PIM devices. The main challenge in efficiently integrating NPU and PIM lies in enabling concurrent operations on both platforms, each addressing a specific kernel type. First, existing PIMs typically operate in a "blocked" mode, allowing only either NPU or PIM to be active at any given time. Second, the inherent dependencies between GEMM and GEMV in LLMs restrict their parallel processing. To tackle these challenges, NeuPIMs is equipped with _dual row buffers_ in each bank, facilitating the simultaneous management of memory read/write operations and PIM commands. Further, NeuPIMs employs a runtime _sub-batch interleaving_ technique to maximize concurrent execution, leveraging batch parallelism to allow two independent sub-batches to be pipelined within a single NeuPIMs device. Our evaluation demonstrates that compared to GPU-only, NPU-only, and a naïve NPU+PIM integrated acceleration approaches, NeuPIMs achieves 3×, 2.4× and 1.6× throughput improvement, respectively.

![[Pasted image 20260110111459.png]]

![[Pasted image 20260110111514.png]]






[T. Zhang](https://spiral.ece.cmu.edu/pub-spiral/submitfilter.jsp?order=author&order=year&author=320) and [Franz Franchetti](https://spiral.ece.cmu.edu/pub-spiral/submitfilter.jsp?order=author&order=year&author=8) (Proc. SRC TECHCON, 2025)  
### **Towards an End-to-End Processing-in-DRAM Acceleration of Spectral Library Search**
- https://spiral.ece.cmu.edu/pub-spiral/pubfile/2025_TECHCON_Tianyun_388.pdf

