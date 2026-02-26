Overview

Quantized CNN Accelerator IP for Edge AI is an open-source, vendor-agnostic FPGA IP core implementing a fully parameterized INT8 convolutional neural network accelerator.

The design targets real-time edge inference for 128×128 image inputs and is optimized for:

Deterministic throughput (1 pixel per clock)

Fully pipelined streaming architecture

On-chip memory reuse

Low external memory bandwidth

FPGA portability across vendors

The IP is written in synthesizable Verilog and designed for integration into embedded FPGA SoCs or discrete FPGA platforms.

Project Objectives

Provide a reusable INT8 CNN accelerator IP core

Achieve 1 pixel per clock streaming throughput

Support 128×128 image processing in real time

Enable parameterized scaling (kernel size, channels, depth)

Maintain vendor-neutral synthesis compatibility

Support open-source FPGA toolchains

Facilitate FOSS hardware incubation

Architecture

The accelerator follows a streaming, fully pipelined architecture with line-buffer-based convolution.

Text-Based Block Diagram
                +----------------------+
AXI-Stream In ->| Input Buffer         |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | Line Buffers         |
                | (Sliding Window)     |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | MAC Array (INT8xINT8)|
                | Parallel PE Units    |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | Accumulator (INT32)  |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | ReLU / Activation    |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | Quantizer (INT8)     |
                +----------+-----------+
                           |
                           v
AXI-Stream Out <-+----------------------+
                 | Output Buffer        |
                 +----------------------+
Configuration

The accelerator is fully parameterized.

Example Verilog Parameter Configuration
module cnn_accelerator #(
    parameter IMG_WIDTH      = 128,
    parameter IMG_HEIGHT     = 128,
    parameter DATA_WIDTH     = 8,     // INT8
    parameter ACC_WIDTH      = 32,    // INT32 accumulation
    parameter KERNEL_SIZE    = 3,
    parameter IN_CHANNELS    = 8,
    parameter OUT_CHANNELS   = 16,
    parameter PE_COUNT       = 16
)(
    input  wire clk,
    input  wire rst_n,
    input  wire [DATA_WIDTH-1:0] s_axis_tdata,
    input  wire s_axis_tvalid,
    output wire s_axis_tready,
    output wire [DATA_WIDTH-1:0] m_axis_tdata,
    output wire m_axis_tvalid,
    input  wire m_axis_tready
);
Data Precision
Stage	Precision	Description
Input Feature	INT8	Quantized activation
Weights	INT8	Signed 8-bit
MAC Operation	INT8×INT8	Parallel multiply
Accumulation	INT32	Prevent overflow
Activation	INT8	ReLU + scaling
Output Feature	INT8	Re-quantized output
Performance Targets

Target configuration example:

Parameter	Value
Image Size	128 × 128
Throughput	1 pixel/clock
Clock Frequency	100 MHz
Latency	Pipeline dependent
Data Precision	INT8
Example Throughput Calculation

128 × 128 = 16,384 pixels

1 pixel per clock

100 MHz clock

Processing time per frame:

16,384 cycles / 100 MHz = 163.84 µs per frame

This enables real-time edge inference with substantial headroom for multi-layer CNN execution.

Memory Architecture

The design minimizes external memory access through on-chip reuse:

Line buffers implemented using BRAM

Weight storage in dual-port BRAM

Partial sum registers for local accumulation

Optional external DRAM interface for deep networks

Memory Strategy

Sliding window reuse

Channel-wise tiling

Weight prefetch buffering

Burst-friendly streaming

Streaming Interface

The accelerator uses AXI-Stream-compatible handshake signaling.

Example Interface
// Input Stream
input  wire [7:0]  s_axis_tdata;
input  wire        s_axis_tvalid;
output wire        s_axis_tready;

// Output Stream
output wire [7:0]  m_axis_tdata;
output wire        m_axis_tvalid;
input  wire        m_axis_tready;

Handshake follows standard ready/valid protocol.

Repository Structure
quantized-cnn-accelerator-ip/
│
├── rtl/
│   ├── cnn_accelerator.v
│   ├── mac_array.v
│   ├── line_buffer.v
│   ├── quantizer.v
│   └── activation_relu.v
│
├── sim/
│   ├── tb_cnn_accelerator.v
│   └── test_vectors/
│
├── synth/
│   ├── yosys/
│   └── nextpnr/
│
├── docs/
│   └── architecture.md
│
├── banner.svg
├── LICENSE
└── README.md
1-Month Development Roadmap
Week 1

Define microarchitecture

Implement parameterized MAC array

Implement sliding window line buffer

Create simulation testbench

Week 2

Integrate convolution pipeline

Implement INT32 accumulator

Add activation + quantization

Verify functional correctness

Week 3

Integrate AXI-Stream interface

Optimize BRAM usage

Add synthesis scripts (Yosys)

Run timing analysis

Week 4

Hardware validation on FPGA board

Throughput benchmarking

Documentation finalization

FOSS release preparation

Toolchain

This project supports open-source FPGA tooling.

Verilator (RTL simulation)

Yosys (Synthesis)

nextpnr (Place & Route)

GTKWave (Waveform viewing)

Vendor tools (Vivado, Quartus, Radiant) may also be used but are not required.

Contributing

Contributions are welcome.

Please:

Fork the repository

Create a feature branch

Add simulation tests

Ensure synthesis passes

Submit a pull request

All contributions must maintain:

Synthesizable RTL

Vendor-neutral compatibility

Clean timing closure

Documented parameters

License

This project is licensed under the MIT License.

See the LICENSE file for details.

Vision

The long-term vision of this project is to:

Democratize hardware AI acceleration

Enable fully open-source edge inference stacks

Provide reusable FPGA CNN infrastructure

Support academic, research, and industrial innovation

Serve as a foundation for future open silicon initiatives
