<p align="center">
  <img src="assets/banner.svg" alt="CNN Accelerator Banner"/>
</p>
📌 Overview

This project implements a fully parameterized, vendor-agnostic CNN accelerator IP core designed for FPGA-based Edge AI inference.

The accelerator supports INT8 quantized 3×3 convolution optimized for 128×128 image processing using a pipelined MAC architecture.

The goal is to provide a reusable, open-source hardware IP block that can be integrated into any FPGA platform without vendor lock-in.

🎯 Project Objectives

Develop reusable RTL for quantized convolution

Support 128×128 real-time inference

Achieve 1 pixel per clock throughput

Maintain vendor independence

Use open-source FPGA toolchains

Publish performance benchmarks

Maintain active weekly development logs

🚧 Development Status

Phase: Active Development
Timeline: 1 Month Structured Roadmap
Update Frequency: Weekly

🏗 Architecture Overview

Input Stream
→ Line Buffer (BRAM)
→ Sliding Window Generator
→ 3×3 Parallel MAC Array
→ Accumulator (INT32)
→ ReLU Activation
→ Output Stream

Fully pipelined for maximum throughput.

⚙ Target Configuration
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

parameter MAC_UNITS      = 9;
🧮 Data Precision
Component	Precision
Input Feature Map	INT8
Weights	INT8
Accumulator	INT32
Output	INT8 (Scaled)
📊 Performance Target

128 × 128 = 16,384 pixels

At 100 MHz:

16,384 cycles ≈ 163.84 µs per output channel

Expected Speedup vs CPU: 20× – 200×

💾 Memory Architecture

BRAM-based Line Buffers

Sliding Window Generator

BRAM Weight Storage

Streaming Output Mode

🔌 Interface
input clk;
input rst;

input input_valid;
input [DATA_WIDTH-1:0] input_data;

output output_valid;
output [DATA_WIDTH-1:0] output_data;

Future:

AXI-Stream

AXI-Lite control interface

📂 Repository Structure
cnn-accelerator-ip/
│
├── rtl/
├── tb/
├── synthesis/
├── docs/
├── benchmarks/
└── progress_logs/
📅 1-Month Development Plan
Week 1

Implement MAC unit

Implement accumulator

Testbench validation

Week 2

Line buffer

Sliding window

Full pipeline integration

Week 3

Pipelining

ReLU

Timing optimization

Week 4

Synthesis

Frequency measurement

Resource utilization report

Benchmark publication

🤝 Contribution

We welcome:

RTL optimizations

Layer additions

Testbench improvements

Documentation updates

Please open an issue before major feature additions.

📜 License

MIT License

⭐ Vision

To build a lightweight, high-performance, open-source CNN accelerator IP core enabling scalable Edge AI deployment on FPGA platforms.

🔥 Why It Happened

It looked wrong because:

You pasted the outer ```markdown wrapper

That forces GitHub to treat everything as code

Just paste normal markdown (like above) and it will render correctly.
