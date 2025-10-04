#  Week 2 – BabySoC Fundamentals & Functional Modelling

## Objective
To build a **solid understanding of SoC fundamentals** and practice **functional modelling** of the **BabySoC** using simulation tools like **Icarus Verilog** and **GTKWave**.

---

## Part 1 – Theory (Conceptual Understanding)

### 🔗 Reference
[Fundamentals of SoC Design – VSD SoC Journey](https://github.com/hemanthkumardm/SFAL-VSD-SoCJourney/tree/main/11.%20Fundamentals%20of%20SoC%20Design)

---

### What is a System-on-Chip (SoC)?
A **System-on-Chip (SoC)** integrates all the essential components of a complete computing system — CPU, memory, I/O peripherals, and communication interfaces — onto a **single silicon die**.  
This integration reduces **power**, **area**, and **cost** while increasing **speed** and **efficiency**.

---

###  Components of a Typical SoC
| Component | Description |
|------------|-------------|
| **CPU (Processor Core)** | Executes program instructions and controls all operations. |
| **Memory (ROM, SRAM, DRAM)** | Stores data, program code, and intermediate results. |
| **Peripherals (UART, SPI, GPIO, Timers)** | Interfaces for communication and control. |
| **Interconnect (Bus or Network-on-Chip)** | The internal backbone that connects CPU, memory, and peripherals. |

---

### Why BabySoC is Used for Learning
The **BabySoC** is a simplified learning model representing the basic architecture of a real SoC.  
It omits complex hardware protocols and focuses on **core functional concepts**, making it ideal for beginners.  

It helps to:
- Understand **data communication** between CPU, memory, and I/O.
- Practice **Verilog simulation** and **waveform analysis**.
- Build intuition before working on **RTL design** and **physical implementation**.

---

###  Role of Functional Modelling
Functional modelling focuses on **verifying the logical behavior** of the system before RTL implementation.  
It defines *what* the system should do, not *how* it is built.

**Purpose:**
- Test system-level logic early.
- Detect bugs before synthesis or layout.
- Validate communication between SoC blocks.

Functional models are written in **behavioral Verilog** and simulated to observe the correctness of data flow.

---

##  Part 2 – Practical: Functional Modelling & Simulation

###  Tools Required
- **Icarus Verilog** → open-source Verilog simulation tool  
- **GTKWave** → waveform viewer

---


## 🛠️ Setup

### 📥 Cloning the Project

```bash
cd ~/VLSI
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC/
```

📂 You’ll see:

* `src/` (modules)
* `images/` (visuals)
* `output/` (simulation results)

---

## 🔧 TLV → Verilog Conversion

Since **RVMYTH** is written in **TL-Verilog (.tlv)**, we need to convert it to Verilog before simulating.

```bash
# Install tools
sudo apt update
sudo apt install python3-venv python3-pip

# Create virtual env
python3 -m venv sp_env
source sp_env/bin/activate

# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

✅ Now you’ll have `rvmyth.v` alongside your other Verilog files.

---

## 🧪 Simulation Flow

### 🔹 Pre-Synthesis Simulation

```bash
mkdir -p output/pre_synth_sim

iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```

📊 View in GTKWave:

```bash
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

### 🔍 Signals to Observe

* ⏱️ **CLK** → Input clock (from PLL)
* 🔄 **reset** → Reset signal
* 🎚 **OUT (DAC)** → Output from DAC (appears digital in sim)
* 🔢 **RV_TO_DAC[9:0]** → 10-bit RVMYTH output → DAC input

---
### 🧠 The Instruction Program Driving BabySoC  

1. Increment counters,
2. Accumulate values into `r17`,
3. Oscillate them to generate analog waveforms,
4. Hold in a final loop.

| #  | Instruction         | Action                  |
| -- | ------------------- | ----------------------- |
| 0  | `ADDI r9, r0, 1`    | r9 = 1 (decrement step) |
| 1  | `ADDI r10, r0, 43`  | r10 = 43 (loop limit)   |
| 2  | `ADDI r11, r0, 0`   | r11 = 0 (counter)       |
| 3  | `ADDI r17, r0, 0`   | r17 = 0 (DAC input)     |
| 4  | `ADD r17, r17, r11` | Accumulate into r17     |
| 5  | `ADDI r11, r11, 1`  | Increment counter       |
| 6  | `BNE r11, r10, -4`  | Repeat until r11=43     |
| 7  | `ADD r17, r17, r11` | r17 += r11              |
| 8  | `SUB r17, r17, r11` | r17 -= r11              |
| 9  | `SUB r11, r11, r9`  | r11--                   |
| 10 | `BNE r11, r9, -4`   | Loop until r11=1        |
| 11 | `SUB r17, r17, r11` | Final adjust            |
| 12 | `BEQ r0, r0, ...`   | Infinite loop           |

---

## 🔄 Execution Timeline

| Phase                   | Registers  | r17 Value          | Behavior           |
| ----------------------- | ---------- | ------------------ | ------------------ |
| **Ramp (Loop1)**        | r11 = 0→42 | r17 = Σ0..42 = 903 | Monotonic increase |
| **Peak**                | r11 = 43   | r17 = 946          | Transient maximum  |
| **Oscillation (Loop2)** | r11 = 43→1 | r17 = 903 ± r11    | Oscillating decay  |
| **Final**               | r11 = 1    | r17 adjusted       | Holds steady       |

---

**Data Flow:**
Instruction Memory → CPU Pipeline → Register r17 → DAC → Analog OUT

---






## 📈 Pre_synth_sim Waveform
<img width="1091" height="790" alt="image" src="https://github.com/user-attachments/assets/1e5eb7ee-cc70-42ae-8fc0-83dff92dacce" />
## 🛠️ Troubleshooting

* ⚠️ **Module Redefinition** → Ensure files are included only once.
* 🛤 **Path Issues** → Use absolute paths if relative ones fail.
* ⏱️ **Waveform Mismatch** → Verify proper GTKWave format selection.

---

### 🚀 Future Work

Currently, the instructions in the CPU are hardcoded. In the future, I plan to develop a firmware workflow that allows writing programs in C. A Python-based tool will then convert the compiled binary into hexadecimal format, which can be loaded into the CPU memory. This will enable the CPU to execute instructions dynamically from memory, rather than relying on hardcoded instructions.


## 📚 Resources

* 🔗 [RISC-V Core – Shivani Shah](https://github.com/shivanishah269/risc-v-core)
* 📘 [SoC Fundamentals – Hemanth Kumar](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/blob/main/11.%20Fundamentals%20of%20SoC%20Design/README.md)
* 🛠 [VSD SoC Journey – Spatha (Day 5)](https://github.com/spatha0011/spatha_vsd-hdp/tree/main/Day5)
* 🌱 [VSDBabySoC – Manili](https://github.com/manili/VSDBabySoC)

