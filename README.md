<p align="center">
  <img src="assets/banner.svg" alt="CNN Accelerator Banner"/>
</p>

---

## 📌 Overview

This project implements a fully parameterized, vendor-agnostic CNN accelerator IP core designed for FPGA-based Edge AI inference.

The accelerator supports INT8 quantized 3×3 convolution optimized for 128×128 image processing using a pipelined MAC architecture.

The goal is to provide a reusable, open-source hardware IP block that can be integrated into any FPGA platform without vendor lock-in.

---

## 🎯 Project Objectives

- Develop reusable RTL for quantized convolution
- Support 128×128 real-time inference
- Achieve 1 pixel per clock throughput (pipelined mode)
- Maintain vendor independence
- Use open-source FPGA toolchains
- Publish performance benchmarks
- Maintain active weekly development logs

---

## 🚧 Development Status

Phase: Active Development  
Timeline: 1 Month Structured Roadmap  
Update Frequency: Weekly  

---

## 🏗 Architecture Overview

The accelerator follows a streaming pipeline architecture:

Input Stream  
→ Line Buffer (BRAM)  
→ Sliding Window Generator  
→ 3×3 Parallel MAC Array  
→ Accumulator (INT32)  
→ ReLU Activation  
→ Output Stream  

The design is fully pipelined to maximize throughput.

---

## ⚙ Target Configuration (Current Development Build)

```verilog
parameter DATA_WIDTH     = 8;
parameter WEIGHT_WIDTH   = 8;
parameter ACC_WIDTH      = 32;

parameter IMG_WIDTH      = 128;
parameter IMG_HEIGHT     = 128;

parameter KERNEL_SIZE    = 3;
parameter STRIDE         = 1;
parameter PADDING        = 1;

parameter IN_CHANNELS    = 1;
parameter OUT_CHANNELS   = 4;  // Reduced for initial testing

parameter MAC_UNITS      = 9;  // 3×3 fully parallel
🧮 Data Precision
Component	Precision
Input Feature Map	INT8
Weights	INT8
Accumulator	INT32
Output	INT8 (Scaled)

Fixed-point arithmetic is used throughout the pipeline.

📊 Performance Target (128×128 Mode)

Total Pixels:
128 × 128 = 16,384

Throughput Goal:
1 Pixel per Clock (after pipeline fill)

At 100 MHz:

16,384 cycles ≈ 163.84 µs per output channel

Expected speedup vs CPU:
20× – 200×

💾 Memory Architecture

BRAM-based Line Buffers

Sliding Window Generator

BRAM Weight Storage

Streaming Output Mode (recommended for smaller FPGAs)

Memory Usage (Single Channel Example):

Input:
16 KB

Output (4 channels):
64 KB

🔌 Interface Specification
Default Streaming Interface
input clk;
input rst;

input input_valid;
input [DATA_WIDTH-1:0] input_data;

output output_valid;
output [DATA_WIDTH-1:0] output_data;

Future Enhancements:

AXI-Stream support

AXI-Lite control registers

📂 Repository Structure
cnn-accelerator-ip/
│
├── rtl/
│   ├── cnn_top.v
│   ├── conv2d.v
│   ├── mac_unit.v
│   ├── line_buffer.v
│   ├── relu.v
│
├── tb/
│   ├── tb_conv.v
│
├── synthesis/
│
├── docs/
│
├── benchmarks/
│
└── progress_logs/
📅 1-Month Development Roadmap
Week 1

Define parameterized top module

Implement MAC unit

Implement accumulator

Create initial testbench

Validate single convolution block

Week 2

Implement line buffer

Implement sliding window generator

Integrate full convolution pipeline

Simulate 128×128 stream

Week 3

Add pipelining stages

Optimize timing

Add ReLU activation

Resource usage review

Week 4

Synthesis and place & route

Frequency analysis

LUT / BRAM / DSP usage measurement

Benchmark vs CPU

Publish performance report

🛠 Toolchain

Open-source FPGA flow recommended:

Verilator (Simulation)

Yosys (Synthesis)

nextpnr (Place & Route)

No vendor lock-in.

🔥 Long-Term Roadmap

Multi-channel parallel execution

Depthwise convolution support

BatchNorm fusion

AXI-Lite control interface

Full CNN layer chaining

SoC integration support

🤝 Contribution Guidelines

We welcome:

RTL optimizations

New layer implementations

Testbench improvements

Documentation updates

Benchmark reports

Please open an issue before major feature additions.

📜 License

MIT License

⭐ Vision

To build a lightweight, high-performance, open-source CNN accelerator IP core that enables scalable Edge AI deployment on FPGA platforms without vendor dependency.


---

This version is:

- Clean
- Professional
- Industry-ready
- FOSS compliant
- Reviewer-friendly
- Structured for 1-month tracking

---

If you want, next I can give:

- 🔥 CONTRIBUTING.md
- 🔥 LICENSE file content
- 🔥 GitHub badges (build, license, status)
- 🔥 Professional abstract for FOSS submission
- 🔥 Architecture diagram markdown section
- 🔥 Benchmark report template

Tell me what you want next.


