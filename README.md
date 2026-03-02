<p align="center">   <img src="assets/banner.svg" alt="CNN Accelerator Banner"/> </p>

##📌 Project Overview

This project implements a fully parameterized, vendor-agnostic INT8 CNN accelerator IP core optimized for 128×128 grayscale images.

The accelerator is written in pure Verilog RTL and designed for FPGA-based edge AI inference using fully open-source toolchains.

It provides a high-throughput, streaming 3×3 convolution engine that can serve as:

✅ A standalone CNN acceleration block

✅ The backbone for task-aware detection pipelines

✅ A reusable IP core for SoC integration (RISC-V / VEGA compatible)

##🎯 Design Goals

⚡ High-throughput streaming architecture (1 pixel per clock)

💾 Low FPGA resource utilization

🧠 INT8 fixed-point quantized inference

🔓 Vendor-independent RTL implementation

🏭 Edge-AI deployment readiness

🔗 Easy integration into heterogeneous SoCs

🎯 Extendable toward task-aware object detection

##🎯 Objectives

🔧 Implement optimized INT8 3×3 convolution accelerator

📡 Support 128×128 grayscale streaming input

⏱ Achieve sustained 1 pixel per clock throughput

♻ Provide reusable parameterized IP core

🛠 Support open-source FPGA toolchain

🧩 Enable future task-conditioning hardware extension

🏗 Accelerator Pipeline
Input Stream (128×128×1 INT8)
        ↓
BRAM Line Buffer (3 rows)
        ↓
Sliding Window Generator (3×3)
        ↓
Parallel MAC Array (9 INT8 MACs)
        ↓
INT32 Accumulator
        ↓
ReLU Activation
        ↓
Output Quantization (INT8 Scaling)
        ↓
Output Stream
✔ Fully pipelined
✔ Streaming architecture
✔ Deterministic latency
✔ Resource-efficient implementation
⚙ Configuration Parameters
parameter DATA_WIDTH   = 8;
parameter WEIGHT_WIDTH = 8;
parameter ACC_WIDTH    = 32;

parameter IMG_WIDTH    = 128;
parameter IMG_HEIGHT   = 128;

parameter KERNEL_SIZE  = 3;
parameter STRIDE       = 1;
parameter PADDING      = 1;

parameter IN_CHANNELS  = 1;   // Grayscale input
parameter OUT_CHANNELS = 8;   // Scalable

parameter MAC_UNITS    = 9;   // 3×3 kernel
🔧 Scalable Design

Configurable output channels

Channel-wise execution for low-resource FPGAs

Ready for multi-layer chaining

Designed for extension into systolic-array mode

🧮 Data Precision
Component	Precision
📥 Input Feature Map	INT8
🧮 Weights	INT8
➕ Accumulator	INT32
📤 Output	INT8 (Scaled + ReLU)
Quantization Formula
output = (accumulator × scale) >> shift

Fixed-point scaling

Hardware-friendly shift-based normalization

No floating-point dependency

📊 Performance Target

For 128×128 grayscale input:

🧮 Total Pixels: 16,384

🚀 Throughput: 1 Pixel per Clock

⏱ At 100 MHz: ~163 µs per output channel

📈 Estimated CPU Speedup: 20× – 200×

🔋 3× lower bandwidth vs RGB input

💾 Memory Architecture

🧱 BRAM-based 3-line buffer

📦 BRAM-based weight storage

🔄 Fully streaming feature map processing

🔀 Channel-wise scalable execution

♻ Double-buffering ready for extension

Key Design Principle

Minimize external memory access by keeping:

Sliding windows on-chip

Partial sums in registers

Weight reuse maximized

🔌 Interface
input clk;
input rst;

input input_valid;
input [DATA_WIDTH-1:0] input_data;

output output_valid;
output [DATA_WIDTH-1:0] output_data;
Planned Interface Extensions

AXI-Stream wrapper

AXI-Lite control registers

DMA-based memory interface

SoC-ready integration layer

🧠 Task-Aware Extension (Future Block)

This accelerator is designed to serve as the CNN backbone for a task-aware detection pipeline.

Planned extended architecture:

Grayscale Image
        ↓
CNN Accelerator (This IP)
        ↓
Feature Map
        ↓
Task Gating Engine
        ↓
Detection Head
        ↓
Most Relevant Object
Benefits of Extension

Feature suppression based on task embedding

Reduced unnecessary computation

Lower latency and power

Edge-optimized task-driven inference

📁 Repository Structure
cnn-accelerator-ip/
│
├── rtl/                # Verilog RTL modules
├── tb/                 # Testbenches
├── synthesis/          # Synthesis scripts
├── docs/               # Architecture documentation
├── benchmarks/         # Performance evaluation
└── progress_logs/      # Weekly development updates
🗓 Development Roadmap
📅 Week 1

MAC unit implementation

INT32 accumulator

Fixed-point validation

Basic testbench

📅 Week 2

Line buffer implementation

Sliding window generator

Pipeline integration

📅 Week 3

ReLU + Quantization block

Timing optimization

Resource utilization analysis

📅 Week 4

Full synthesis

Frequency measurement

Resource utilization report

Benchmark publication

Task-aware extension design draft

🛠 Toolchain

🧪 Verilator (Simulation)

🔨 Yosys (Synthesis)

📍 nextpnr (Place & Route)

Fully compatible with open-source FPGA toolchains.

No vendor lock-in.

🔮 Future Enhancements

8×8 systolic array configuration

Depthwise convolution support

Multi-layer CNN chaining

Task-gated hardware block

AXI-based SoC integration

Sparse compute optimization

Clock gating & power optimization

Multi-channel parallel processing

📜 License

Open-Source (Recommended: MIT / Apache 2.0)

⭐ Vision

To build a scalable, lightweight, open-source CNN accelerator IP core enabling high-performance FPGA-based edge AI deployment — with seamless extensibility toward task-aware object detection and heterogeneous SoC integration.
