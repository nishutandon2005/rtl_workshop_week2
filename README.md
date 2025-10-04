# 🧠 Week 2: BabySoC Fundamentals & Functional Modelling

## 🎯 Objective  
To understand the core concepts of **System-on-Chip (SoC)** design and perform **functional modelling** of the BabySoC using simulation tools like **Icarus Verilog** and **GTKWave**.

---

## 📘 What is a System-on-Chip (SoC)?

A **System-on-Chip (SoC)** is an integrated circuit that combines multiple components of a computer system onto a single silicon chip. Unlike traditional systems where CPU, memory, and peripherals are separate ICs, an SoC integrates everything to achieve high performance, compact design, and low power consumption.  

In essence, an SoC is a **complete computing system** on one chip — capable of executing software, processing data, and interfacing with the outside world.

---

## ⚙️ Components of a Typical SoC

A standard SoC consists of the following key components:

| Component | Description |
|------------|-------------|
| **CPU (Processor Core)** | The brain of the SoC — executes instructions and controls all operations. |
| **Memory (SRAM, ROM, Flash)** | Stores program code and temporary data used during execution. |
| **Peripherals (I/O Interfaces, Timers, UART, GPIO, etc.)** | Allow communication with external devices and sensors. |
| **Interconnect (Bus or Network-on-Chip)** | Connects CPU, memory, and peripherals — enabling data transfer and coordination. |

These blocks work together under a unified clock and power architecture, ensuring efficient operation and communication.

---

## 🧩 Why BabySoC?

**BabySoC** is a **simplified educational model** of a real System-on-Chip.  
It is designed to help learners understand SoC fundamentals **without overwhelming hardware complexity**.  

Key reasons why BabySoC is ideal for learning:
- It models only the **essential SoC components** (CPU core, memory, and a few peripherals).  
- Allows easy **functional simulation** and waveform analysis using open-source tools.  
- Acts as a **step-by-step gateway** to real-world SoC design and verification flows.  
- Provides a clear view of **data paths**, **control signals**, and **module interactions**.

---

## 🧮 Role of Functional Modelling

**Functional modelling** is the first stage in the SoC design process — performed **before RTL design and physical implementation**.  

It helps designers:
- Verify **system-level functionality and data flow**.
- Validate **interface protocols** between modules.  
- Check **logical correctness** before diving into detailed RTL coding.  
- Detect and fix design flaws early — saving time and resources.

Functional modelling with tools like **Icarus Verilog** and **GTKWave** ensures that the BabySoC behaves as intended at the conceptual level before it transitions into the RTL and physical design stages.

---

## 🧭 Summary

| Stage | Purpose |
|--------|----------|
| **Functional Modelling** | Defines high-level SoC behavior and data flow. |
| **RTL Design** | Implements register-level logic of each block. |
| **Physical Design** | Converts the verified RTL into a silicon layout. |

The **BabySoC** bridges the gap between theoretical understanding and hands-on SoC design — making it an essential foundation in the **VSD SoC Journey**.

---

**📂 Reference:**  
[Fundamentals of SoC Design – VSD SoC Journey GitHub Repository](https://github.com/hemanthkumardm/SFAL-VSD-SoCJourney/tree/main/11.%20Fundamentals%20of%20SoC%20Design)
