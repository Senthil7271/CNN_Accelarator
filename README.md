<p align="center">   <img src="assets/banner.svg" alt="CNN Accelerator Banner"/> </p>

## 📌 Project Overview

This project implements a fully parameterized, vendor-agnostic CNN accelerator IP core optimized for INT8 3×3 convolution on 128×128 images.

The accelerator is written in Verilog RTL, designed for FPGA-based edge AI inference, and compatible with open-source FPGA toolchains.

## 🎯 Design Goals

- ⚡ High throughput
- 💾 Low resource utilization
- 🧠 Quantized fixed-point inference
- 🔓 Vendor independence
- 🏭 Edge deployment readiness

## 🎯 Objectives

- 🔧 Implement INT8 3×3 convolution accelerator
- 📡 Support 128×128 streaming input
- ⏱ Achieve 1 pixel per clock throughput
- ♻ Provide reusable parameterized IP core
- 🛠 Support open-source FPGA toolchain
- 🔗 Enable easy SoC integration

## 🏗 Accelerator Pipeline




```

Input Stream
↓
Line Buffer (BRAM)
↓
Sliding Window Generator
↓
3×3 Parallel MAC Array
↓
INT32 Accumulator
↓
ReLU Activation
↓
Output Stream

```

- ✔ Fully pipelined
- ✔ Configurable parallelism
- ✔ Resource-efficient design

## ⚙ Configuration Parameters

```verilog
parameter DATA_WIDTH   = 8;
parameter WEIGHT_WIDTH = 8;
parameter ACC_WIDTH    = 32;

parameter IMG_WIDTH    = 128;
parameter IMG_HEIGHT   = 128;

parameter KERNEL_SIZE  = 3;
parameter STRIDE       = 1;
parameter PADDING      = 1;

parameter IN_CHANNELS  = 1;
parameter OUT_CHANNELS = 4;

parameter MAC_UNITS    = 9;

```

## 🧮 Data Precision

| Component | Precision |
| --- | --- |
| 📥 Input Feature Map | INT8 |
| 🧮 Weights | INT8 |
| ➕ Accumulator | INT32 |
| 📤 Output | INT8 (Scaled) |

## 📊 Performance Target

For 128×128 input:

* 🧮 Total Pixels: 16,384
* 🚀 Throughput: 1 Pixel per Clock
* ⏱ At 100 MHz: ~163 µs per output channel
* 📈 Estimated CPU Speedup: 20× – 200×

## 💾 Memory Architecture

* 🧱 BRAM-based line buffers
* 📦 BRAM weight storage
* 🔄 Streaming feature map processing
* 🔀 Channel-wise execution for scalability

## 🔌 Interface

```verilog
input clk;
input rst;

input input_valid;
input [DATA_WIDTH-1:0] input_data;

output output_valid;
output [DATA_WIDTH-1:0] output_data;

```

## 🔮 Future Extensions

* AXI-Stream interface
* AXI-Lite control registers
* Multi-layer CNN chaining

## 📁 Repository Structure

```
cnn-accelerator-ip/
│
├── rtl/
├── tb/
├── synthesis/
├── docs/
├── benchmarks/
└── progress_logs/

```

## 🗓 Development Roadmap

### 📅 Week 1

* MAC unit
* Accumulator
* Basic testbench

### 📅 Week 2

* Line buffer
* Sliding window
* Pipeline integration

### 📅 Week 3

* Timing optimization
* ReLU integration
* Resource analysis

### 📅 Week 4

* Full synthesis
* Frequency measurement
* Resource utilization report
* Benchmark publication

## 🛠 Toolchain

* 🧪 Verilator (Simulation)
* 🔨 Yosys (Synthesis)
* 📍 nextpnr (Place & Route)

Vendor lock-in free.

## 📜 License

MIT License

## ⭐ Vision

To build a scalable, lightweight, open-source CNN accelerator IP core enabling high-performance FPGA-based Edge AI deployment without vendor dependency.

