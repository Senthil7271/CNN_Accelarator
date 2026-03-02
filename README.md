<p align="center">   <img src="assets/banner.svg" alt="CNN Accelerator Banner"/> </p>


[![License](https://img.shields.io/badge/License-MIT/Apache%202.0-blue.svg)](#-license)
[![Toolchain](https://img.shields.io/badge/Toolchain-Verilator%20|%20Yosys%20|%20nextpnr-orange.svg)](#-toolchain)
[![FPGA](https://img.shields.io/badge/FPGA-Vendor--Agnostic-green.svg)](#-design-goals)

A high-throughput, streaming 3×3 convolution engine designed in pure Verilog RTL. This IP core is optimized for 128×128 grayscale edge-AI inference and is compatible with fully open-source FPGA toolchains.

---

## 📌 Project Overview

This project implements a vendor-agnostic CNN acceleration block capable of serving as a standalone processing unit or the backbone for complex, task-aware detection pipelines (RISC-V/VEGA compatible).

### 🎯 Design Goals
* **⚡ High Throughput:** Streaming architecture achieving 1 pixel per clock cycle.
* **💾 Resource Efficient:** Low FPGA footprint via BRAM-based line buffering.
* **🧠 INT8 Quantized:** Optimized fixed-point inference (INT8 weights/data, INT32 accumulation).
* **🔓 Open Source:** Designed for `Yosys`, `nextpnr`, and `Verilator`—no vendor lock-in.
* **🧩 Scalable:** Parameterized RTL for easy adaptation to different image sizes or channel depths.

---

## 🏗 Accelerator Architecture

The IP utilizes a classic line-buffer sliding window approach to maximize data reuse and minimize external memory bandwidth.



### Pipeline Stages
1.  **Input Stream:** 128×128×1 INT8 pixels.
2.  **BRAM Line Buffer:** Stores 3 rows of the image to enable sliding window access.
3.  **Sliding Window Generator:** Extracts 3×3 pixel neighborhoods every clock cycle.
4.  **Parallel MAC Array:** 9 concurrent INT8 Multiply-Accumulate operations.
5.  **ReLU Activation:** Zero-clipping for non-linearity.
6.  **Output Quantization:** Scaling and bit-shifting back to INT8.

---

## ⚙ Configuration Parameters

The core is fully customizable via top-level parameters:

| Parameter | Default | Description |
| :--- | :--- | :--- |
| `DATA_WIDTH` | 8 | Input feature map bit-width |
| `WEIGHT_WIDTH` | 8 | Kernel weight bit-width |
| `ACC_WIDTH` | 32 | Intermediate accumulation width |
| `IMG_WIDTH` | 128 | Image width (adjustable) |
| `KERNEL_SIZE` | 3 | Supported kernel size |
| `OUT_CHANNELS`| 8 | Number of output filters |

---

## 🧮 Data Precision & Performance

To avoid costly floating-point logic, we utilize a hardware-friendly quantization formula:

$$output = (accumulator \times scale) \gg shift$$

### 📊 Performance Targets (at 100 MHz)
* **Total Pixels:** 16,384
* **Throughput:** ~163 µs per output channel
* **Clock Speed:** Target 100 MHz on Lattice ECP5 / iCE40 or Xilinx/Intel equivalents.
* **Efficiency:** 20× – 200× speedup over software-only CPU inference.

---

## 🔌 Interface
The IP uses a simple streaming interface, making it easy to wrap with AXI-Stream for SoC integration.

```verilog
input clk;
input rst;

// Input Stream
input input_valid;
input [DATA_WIDTH-1:0] input_data;

// Output Stream
output output_valid;
output [DATA_WIDTH-1:0] output_data;
```
# 🚀 CNN Accelerator IP Development Roadmap

## 🗓 Development Schedule

### 📅 Week 1: Core Arithmetic
- [ ] Implement **INT8 MAC** units.
- [ ] Design **INT32 accumulator** logic.
- [ ] Basic unit testbench for fixed-point validation.

### 📅 Week 2: Memory & Flow
- [ ] **BRAM Line Buffer** implementation.
- [ ] Sliding window generator logic.
- [ ] Full pipeline integration.

### 📅 Week 3: Optimization
- [ ] **ReLU & Quantization** block.
- [ ] Timing closure and critical path analysis.
- [ ] Resource utilization reports.

### 📅 Week 4: Deployment
- [ ] Full synthesis with open-source tools.
- [ ] Benchmark publication.
- [ ] Task-aware hardware extension draft.

---

## 🛠 Toolchain Support
This project intentionally avoids proprietary IP cores to ensure compatibility with an entirely open-source flow:
* **Simulation:** [Verilator](https://www.veripool.org/verilator/)
* **Synthesis:** [Yosys](https://yosyshq.net/yosys/)
* **Place & Route:** [nextpnr](https://github.com/YosysHQ/nextpnr)

---

## 📁 Repository Structure
```plaintext
cnn-accelerator-ip/
├── rtl/                # Verilog RTL modules
├── tb/                 # Testbenches (Verilator / CocoTB)
├── synthesis/          # Synthesis scripts (Yosys)
├── docs/               # Technical documentation
└── progress_logs/      # Weekly updates
```
# 📜 License
This project is dual-licensed under the **MIT** and **Apache 2.0 License**. You may choose the license that best fits your project needs. See the `LICENSE` file for the full text.

---

# 🔮 Vision
To empower edge devices with **lightweight, high-performance, and open-source AI hardware** that bridges the gap between raw data and task-aware intelligence.



We believe in a future where:
* **Edge AI** is accessible without proprietary vendor lock-in.
* **Hardware-Software co-design** is transparent and verifiable.
* **Low-power acceleration** enables sophisticated defect detection and computer vision on the smallest silicon footprints.
