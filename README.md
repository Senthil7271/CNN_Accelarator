📌 Project Overview

This project implements a fully parameterized, vendor-agnostic CNN accelerator IP core optimized for INT8 3×3 convolution on 128×128 images.

The accelerator is written in Verilog RTL, designed for FPGA-based edge AI inference, and compatible with open-source FPGA toolchains.

The design goal is to balance:

⚡ High throughput

💾 Low resource utilization

🧠 Quantized fixed-point inference

🔓 Vendor independence

🏭 Edge deployment readiness

🎯 Objectives

Implement INT8 3×3 convolution accelerator

Support 128×128 streaming input

Achieve 1 pixel per clock throughput

Provide reusable parameterized IP core

Support open-source FPGA toolchain

Enable easy integration into SoC systems

🏗 Accelerator Pipeline
Input Stream
        │
        ▼
Line Buffer (BRAM)
        │
        ▼
Sliding Window Generator
        │
        ▼
3×3 Parallel MAC Array
        │
        ▼
INT32 Accumulator
        │
        ▼
ReLU Activation
        │
        ▼
Output Stream
⚙️ Configuration Parameters
parameter DATA_WIDTH     = 8;
parameter WEIGHT_WIDTH   = 8;
parameter ACC_WIDTH      = 32;

parameter IMG_WIDTH      = 128;
parameter IMG_HEIGHT     = 128;

parameter KERNEL_SIZE    = 3;
parameter STRIDE         = 1;
parameter PADDING        = 1;

parameter IN_CHANNELS    = 1;
parameter OUT_CHANNELS   = 4;

parameter MAC_UNITS      = 9;  // 3×3 Fully Parallel
🧮 Data Precision
Component	Precision
Input Feature Map	INT8
Weights	INT8
Accumulator	INT32
Output	INT8 (Scaled)

Quantization uses fixed-point arithmetic with post-accumulation scaling.

📊 Performance Target

For 128×128 input:

Total Pixels: 16,384

At 100 MHz:

Metric	Value
Throughput	1 Pixel / Clock
Latency (1 Channel)	~163 µs
Estimated Speedup vs CPU	20× – 200×
💾 Memory Architecture

BRAM-based Line Buffers

BRAM Weight Storage

Streaming Feature Map Processing

Channel-wise execution for scalability

Memory Requirements (Example):

Component	Size
Input (128×128×8-bit)	16 KB
Output (4 channels)	64 KB
🔌 Interface Specification
input clk;
input rst;

input input_valid;
input [DATA_WIDTH-1:0] input_data;

output output_valid;
output [DATA_WIDTH-1:0] output_data;

Future Extensions:

AXI-Stream Interface

AXI-Lite Control Registers

Multi-layer CNN chaining

🛠 Toolchain Compatibility
Stage	Tool
Simulation	Verilator
Synthesis	Yosys
Place & Route	nextpnr

Fully compatible with open-source FPGA flow.

📁 Repository Structure
cnn-accelerator-ip/
│
├── README.md
│
├── rtl/
│   ├── cnn_top.v
│   ├── conv2d.v
│   ├── mac_unit.v
│   ├── line_buffer.v
│   └── relu.v
│
├── tb/
│   └── tb_conv.v
│
├── synthesis/
│
├── docs/
│
└── progress_logs/
📅 Development Roadmap (1 Month Plan)
Week 1

MAC Unit Implementation

Accumulator Design

Basic Testbench

Week 2

Line Buffer

Sliding Window Generator

Pipeline Integration

Week 3

Timing Optimization

ReLU Integration

Resource Analysis

Week 4

Full Synthesis

Frequency Measurement

LUT / BRAM / DSP Report

Benchmark Publication

⚡ Edge Optimization Strategies

INT8 quantization

Pipelined MAC architecture

Parallel 3×3 convolution

Streaming interface

Channel-wise execution

BRAM-based buffering

📦 Deliverables

Parameterized RTL code

Testbench and simulation results

Synthesis reports

Performance benchmarks

Weekly progress documentation

📜 License

MIT License

⭐ Vision

To build a scalable, lightweight, open-source CNN accelerator IP core that enables high-performance FPGA-based Edge AI deployment without vendor lock-in.
