# Standard AMD Workflow to Put AI into an FPGA

> **Legend:** 
> - ✅ **[correct]** — statement is accurate
> - ⚠️ **[doubtable]** — plausible but imprecise, outdated, or not universally true
> - ❌ **[incorrect]** — statement is factually wrong

---

The AMD ecosystem provides four major tools for deploying AI on FPGA/SoC hardware: ✅ **[correct]**

| Tool | Role |
|------|------|
| **Vitis AI** | AI-model quantization, compilation, and runtime (the AI-specific pipeline) |
| **Vitis HLS** | High-Level Synthesis — C/C++ → RTL for custom operators the DPU cannot handle |
| **Vivado** | Traditional FPGA design: RTL synthesis, place-and-route, IP integration, bitstream generation |
| **Vitis** | Unified software platform: host application, PS–PL linking, platform creation, system debug |

> **Typical target hardware:** AMD Zynq MPSoC / Zynq UltraScale+ (e.g., ZCU102, ZCU104, Kria SOM) and Versal ACAP boards. These combine an ARM processing system (PS) with FPGA programmable logic (PL), allowing the DPU to sit in the PL while Linux and VART run on the PS. ✅ **[correct]**

---

## Workflow Overview

```
PyTorch / TensorFlow
        │
        ▼
   ①  Export to ONNX (or float model)
        │
        ▼
   ②  Vitis AI Quantizer   →  INT8 model
        │
        ▼
   ③  Vitis AI Compiler    →  .xmodel file
        │
        ▼
   ④  Vivado + Vitis       →  DPU bitstream + platform
        │
        ▼
   ⑤  VART / Vitis AI Library   →  host application (C++ / Python)
```

✅ **[correct]** — This is the canonical AMD Vitis AI workflow.

---

## Step 1 — Train & Export

- Train your model in **PyTorch** or **TensorFlow** (float32). ✅ **[correct]**
- Export the trained model to **ONNX** (Open Neural Network Exchange), or pass the float model directly into Vitis AI if the framework is natively supported. ✅ **[correct]** — Vitis AI accepts both ONNX and native TensorFlow/PyTorch formats.

## Step 2 — Quantization (Vitis AI Quantizer)

- Convert weights and activations from **float32 → INT8**. ✅ **[correct]**
- Vitis AI performs **post-training quantization (PTQ)** using a small calibration dataset (100–1000 unlabeled samples) to analyse activation distributions. ✅ **[correct]**
- If accuracy loss is unacceptable, **quantization-aware training (QAT)** can fine-tune the model with simulated quantization — use **Brevitas** (PyTorch) or **QKeras** / **TensorFlow Model Optimization**. ✅ **[correct]**


## Step 3 — Compilation (Vitis AI Compiler)

- Feed the quantized model into the **Vitis AI Compiler**. ✅ **[correct]**
- The compiler maps the model graph onto the target DPU architecture (specified by an `arch.json` file). It performs operator fusion (e.g., BatchNorm folded into Convolution), data-layout transformations, and instruction scheduling across DPU cores. ✅ **[correct]**
- Output: a **`.xmodel`** file — the compiled instruction stream the DPU executes at runtime. ✅ **[correct]**

## Step 4 — Hardware Platform (Vivado + Vitis)

This is the hardware-design step that creates the FPGA bitstream containing the DPU: ✅ **[correct]**

1. **Vivado** — integrate the **DPU IP core** into a block design. Configure the DPU (number of cores, BRAM/URAM allocation, supported convolution types). Add other necessary IP: clocking, resets, AXI interconnects, MIPI/CSI camera receivers, etc. Run synthesis, implementation (place-and-route), and generate the **bitstream** (`.bit`) and hardware hand-off file (`.xsa`). ✅ **[correct]**
2. **Vitis** — import the `.xsa` from Vivado to create a **platform project**. The platform packages the hardware design with the software stack (FSBL, PMU firmware, Linux kernel, device tree) so the host application can communicate with the DPU. ✅ **[correct]**

> **Vitis HLS** (optional / advanced): If your model contains operators the DPU does not natively support, write a custom accelerator in C/C++, synthesize it to RTL via Vitis HLS, and integrate the resulting IP block into the Vivado design alongside the DPU. ✅ **[correct]** — For most standard CNNs (ResNet, YOLO, MobileNet, VGG, etc.) this is unnecessary — the DPU handles them directly. ✅ **[correct]**

## Step 5 — Deployment & Host Application

### Flash the Bitstream
Load the DPU bitstream onto the board. Common methods: ✅ **[correct]**

| Method | Use Case |
|--------|----------|
| **Vivado Hardware Manager** (JTAG over USB) | Development / debugging |
| **SD card / eMMC boot** | Standalone embedded deployment (Zynq MPSoC, Kria) |
| **QSPI flash** (`program_flash` via Vivado) | Production, non-volatile boot |
| **Ethernet / PCIe** | Data-centre accelerator cards (Alveo) |

### Write the Host Application
On the ARM processing system, use the **Vitis AI Runtime (VART)**: ✅ **[correct]**

- **C++ or Python API** to load the `.xmodel`, submit inference jobs to the DPU asynchronously, and retrieve results. ✅ **[correct]**
- **Vitis AI Library** provides higher-level wrappers with pre-built pre-processing (resize, normalise) and post-processing (NMS, softmax) pipelines for common tasks. ✅ **[correct]**
- Typical loop: capture frame from camera (MIPI) → pre-process → DPU inference → post-process → act on result. ✅ **[correct]**

```
┌──────────────────────────────────────┐
│  Processing System (ARM / Linux)     │
│  ┌────────────────────────────────┐  │
│  │  Host App (C++ / Python)       │  │
│  │  VART / Vitis AI Library       │  │
│  └───────────┬────────────────────┘  │
│              │ AXI bus                │
│  ┌───────────▼────────────────────┐  │
│  │  Programmable Logic (FPGA)     │  │
│  │  ┌──────┐  ┌──────┐  ┌──────┐ │  │
│  │  │ DPU  │  │ MIPI │  │Custom│ │  │
│  │  │ Core │  │ CSI  │  │ HLS  │ │  │
│  │  └──────┘  └──────┘  └──────┘ │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

✅ **[correct]** — Accurate PS/PL architecture diagram for Zynq/Versal platforms.

---

## Tool Summary

| Step | Tool | Input | Output |
|------|------|-------|--------|
| Train | PyTorch / TensorFlow | Dataset | float32 model |
| Export | `torch.onnx` / `tf2onnx` | float32 model | ONNX |
| Quantize | Vitis AI Quantizer | float32 model + calibration data | INT8 model |
| Compile | Vitis AI Compiler | INT8 model + `arch.json` | `.xmodel` |
| Hardware | Vivado | DPU IP + block design | bitstream (`.bit`) + `.xsa` |
| Platform | Vitis | `.xsa` | platform (boot images, device tree) |
| Program | Vivado Hardware Manager / SD boot | bitstream | running DPU on FPGA |
| Host App | VART / Vitis AI Library | `.xmodel` + sensor data | inference results |

> **Environment note:** Vitis AI tools ship as **Docker containers** (CPU and GPU variants). ✅ **[correct]** Vivado and Vitis are native Linux/Windows installs. ✅ **[correct]** The DPU IP core is included with Vitis AI and instantiated inside Vivado. ✅ **[correct]**

---

## External Resources

- [Advanced FPGA Course: Verilog Based Robotics & Signal Processing](https://www.youtube.com/playlist?list=PLfPwG72dAOx6gkqrrZVdr63EaizecO0Cg)
- [hls4ml](https://fastmachinelearning.org/hls4ml/) — translate machine learning models directly into synthesizable VHDL or Verilog
- [PyTorch AI on FPGA — FINN Workflow Tutorial](https://hugobrh.dev/posts/PY2FPGA/)
- [Python to FPGA](https://github.com/0BAB1/tutorial-snippets/tree/main/8%20Python%20to%20FPGA)
- [Educational Platform for FPGA Accelerated AI in Robotics](https://github.com/nhma20/FPGA_AI)
- [Workflow of deploying a model (AMD) — Vitis AI 3.5 docs](https://xilinx.github.io/Vitis-AI/3.5/html/docs/workflow-model-deployment.html)
- **OpenCL** ⚠️ **[doubtable]** — orphaned bullet; presumably a reference to AMD's XRT/OpenCL flow for Alveo FPGA accelerators, but the context is missing. Needs clarification or removal.

---

## AI Module Comparison

> ⚠️ **[doubtable]** — The TOPS and power figures below are **vendor-marketing numbers** measured under different conditions (peak vs. sustained, INT8 vs. mixed precision, with vs. without sparsity). Use them for rough comparison only; real-world performance depends heavily on workload, batch size, and thermal constraints.

| Platform / Device | Peak AI Performance (TOPS) | Power Consumption | Primary Use Case & Architecture | Verdict |
|---|---|---|---|---|
| **NVIDIA Jetson AGX Orin** | Up to 275 TOPS (INT8) | 15W – 60W | High-end robotics, autonomous navigation. Integrates CPU, Ampere GPU, and Deep Learning Accelerators (DLA). | ✅ **[correct]** |
| **NVIDIA Jetson Orin Nano (Super)** | ~~Up to 40 TOPS~~ | 7W – 25W | Entry-level embedded AI and smart cameras. Offers a lower-power CUDA entry point. | ❌ **[incorrect]** — The "Super" variant (announced Dec 2024) delivers **up to 67 INT8 TOPS** (8 GB SKU), not 40. The original Orin Nano (non-Super) was 40 TOPS. Update this figure if you mean the Super. |
| **Raspberry Pi 5 + Hailo-8 AI HAT** | ~13 to 26 TOPS | 10W – 15W | Low-power field robots. Relies on a separate Hailo accelerator (Hailo-8L = 13 TOPS, Hailo-8 = 26 TOPS) for edge inference. | ✅ **[correct]** |
| **Intel NUC (Core Ultra / Arc)** | ~20 to 40+ TOPS (NPU + GPU) | 28W – 65+W (system) | Industrial edge / edge server. Allows full x86 compatibility and PCIe expansion. | ⚠️ **[doubtable]** — TOPS vary massively by generation: Meteor Lake NPU ≈ 11 TOPS, Lunar Lake NPU ≈ 48 TOPS, Arrow Lake NPU ≈ 13 TOPS. "20–40+ TOPS" is a reasonable ballpark for Meteor Lake + Arc GPU combined, but Lunar Lake NUCs can far exceed this. Specify the exact CPU generation. |
| **Axelera Metis AI** | Up to 214 TOPS | 20W – 40W | Based on in-memory computing (PIM) and RISC-V; optimized for ultra-low latency at high TOPS. | ✅ **[correct]** — Matches Axelera's published Metis AIPU specs. |
| **EdgeCortix SAKURA** | Up to 60 TOPS | Under 10W | Highly efficient FIM (Fabric in Memory) architecture; targets low-power smart city and vision applications. | ✅ **[correct]** — Matches EdgeCortix SAKURA-I published specs. |
| **SiMa.ai MLSoC** | Up to 50+ TOPS | Under 5W | Purpose-built for edge AI; integrated CV/vision pipeline with software-programmable NPU. | ⚠️ **[doubtable]** — The "50+ TOPS" claim is plausible, but **"under 5W" is aggressive** for the full SoC at peak throughput. SiMa.ai's public materials cite ~5–10W typical board power depending on workload. Verify against a specific SKU datasheet. |

> 📝 **Note:** The original entry for SiMa.ai was truncated at "Purpose-bui". The description above has been completed based on publicly available SiMa.ai materials.

> 🔗 **Reference link:** [Google share — AI module comparison](https://share.google/aimode/L7ZSdQgz3HyC4D2pm) ⚠️ **[doubtable]** — This URL does not match the standard `share.google.com` format. It may be an internal/corporate Google link, a typo, or an expired share. Verify it resolves before relying on it.
