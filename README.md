# Parallel Canny Edge Detection (OpenMP)

## Overview

This project implements and benchmarks **parallel versions of the Canny Edge Detection algorithm** using **C++ and OpenMP**. Multiple synchronization strategies are compared to study their impact on **correctness, performance, speedup, and efficiency**.

The work focuses on accelerating the computationally expensive stages of Canny while preserving output quality, and it includes both correct and intentionally incorrect (race-condition) implementations for analysis.

---

## Problem Statement

Canny Edge Detection involves several convolution-heavy stages that are time‑consuming when executed sequentially. Naïve parallelization introduces **race conditions** due to shared variables, which can corrupt results.

This project explores how different OpenMP synchronization techniques affect:

* Correctness of results
* Execution time
* Scalability across thread counts

---

## Implemented Versions

* **Sequential implementation** (baseline)
* **Parallel implementations** using OpenMP:

  * Atomic
  * Critical section
  * Reduction
  * Race condition (for demonstration and comparison)

---

## Parallelized Pipeline Stages

The following stages were parallelized:

1. Gaussian Blur
2. Sobel Filtering
3. Non‑Maximum Suppression
4. Double Thresholding

⚠️ **Hysteresis** remains sequential due to inherent data dependencies between pixels.

---

## Shared Variables & Synchronization

During Sobel filtering, the following shared variables caused race conditions:

* Gradient magnitude accumulation
* Strong edge count
* Maximum gradient value

Each synchronization method was applied to resolve these conflicts, allowing a direct comparison of overhead and scalability.

---

## Performance Evaluation

* Multiple image sizes were tested
* Benchmarks were run with different thread counts
* Each configuration was executed multiple times to ensure stability

### Key Findings

* **Reduction** achieved the best overall performance, especially at **4 threads**
* **Critical sections** introduced significant overhead
* Performance degraded at higher thread counts due to CPU and synchronization limits

---

## Project Structure

```
parallel-canny-edge-detection/
├── code/
│   ├── Serial_Code.cpp
│   ├── Atomic_Code.cpp
│   ├── Critical_Code.cpp
│   ├── Reduction_Code.cpp
│   └── Race_Code.cpp
│
├── outputs/
│   ├── Serial_Output.txt
│   ├── Atomic_Output.txt
│   ├── Atomic_Default.txt
│   ├── Critical_Output.txt
│   ├── Critical_Default.txt
│   ├── Reduction_Output.txt
│   ├── Reduction_Default.txt
│   └── Race_5_Run_Outputs.txt
│
├── images/
│   ├── stonehse_gray.png
│   └── stonehse_edges.png
│
├── report/
│   └── Parallel_Project_Report.pdf
│
├── README.md
└── requirements.txt
```

---

## Tools & Environment

* Language: C++
* Parallel API: OpenMP
* Compiler: GCC (`-O3 -fopenmp`)
* OS: Ubuntu Linux

---

## Learning Outcomes

* Understanding race conditions in shared-memory parallelism
* Comparing synchronization overheads in OpenMP
* Evaluating parallel speedup and efficiency
* Applying parallel computing concepts to real image processing pipelines

---

## Future Work

* GPU implementation (CUDA)
* Task-based parallelism
* Parallel hysteresis using region-based approaches
