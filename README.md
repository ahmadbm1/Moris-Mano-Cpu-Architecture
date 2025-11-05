# 📘 Moris Mano CPU Architecture (Logisim Evolution)

[![Made with ❤️ for Education](https://img.shields.io/badge/Made%20with-%E2%9D%A4%EF%B8%8F%20for%20Education-blue)](https://github.com)
[![Tool](https://img.shields.io/badge/Tool-Logisim%20Evolution-informational)](https://github.com/reds-heig/logisim-evolution)
[![Status](https://img.shields.io/badge/Status-Learning%20Project-brightgreen)](#)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](#license)

A simulated implementation of the classic **Morris Mano Basic Computer** built in **Logisim Evolution**.

---

## 🔎 Overview
This project recreates the CPU architecture described by **Morris Mano** in _Computer System Architecture_.  
It models the **Basic Computer** with a clear, modular design: registers, ALU, control logic, memory, and I/O.

### Why this repo?
- Friendly, step-by-step layout for students
- Clean signal naming & grouped buses
- Helpful probes and LEDs for debugging
- Example ROM programs to test the full instruction cycle

---

## 📁 Project Structure
```
/ ── MorisManoCPU.circ        # Main Logisim Evolution circuit
│
├── ROM.hex                   # Sample program (memory init)
│
│
└── README.md                 # You are here ✨
```

---

## 🧩 Architectural Blocks
- **Registers**: AC, DR, IR, TR, PC, AR, OUTR, INPR, SC
- **ALU**: Add, AND, complement, increment, shifts
- **Control Unit**: Sequence counter (T0–T15), micro-operations
- **Memory**: 4K×16 (typical), hex-loaded from `ROM.hex`
- **FlipFlops**: I, S, E, R, IEN, FGI, FGO

> Tip: Use Logisim’s **Tick Frequency** to slow down and observe micro-ops in real time.

---

## ⏱ Instruction Cycle (Fetch → Decode → Execute)
**Fetch**
```
T0: AR ← PC
T1: IR ← M[AR], PC ← PC + 1
T2: Decode IR(14–12)  # opcode
```

**Execute**
- Depends on instruction class: **Memory-Reference**, **Register-Reference**, or **I/O**.

See `docs/micro-ops.md` for a full micro-op table per instruction (or extend this README if you prefer one file).

---

## 📜 Instruction Set (Classic Mano)
### Memory-Reference (I bit may indicate indirect)
| Mnemonic | Opcode | Effect (high-level)           |
|---|---|---|
| AND     | 0000   | AC ← AC ∧ M[addr]              |
| ADD     | 0001   | AC ← AC + M[addr]              |
| LDA     | 0010   | AC ← M[addr]                   |
| STA     | 0011   | M[addr] ← AC                   |
| BUN     | 0100   | PC ← addr                      |
| BSA     | 0101   | M[addr] ← PC; PC ← addr+1      |
| ISZ     | 0110   | M[addr] ← M[addr]+1; skip if 0 |

### Register-Reference (when opcode = 111 & I=0)
`CLA, CLE, CMA, CME, CIR, CIL, INC, SPA, SNA, SZA, SZE, HLT`

### I/O Instructions (when opcode = 111 & I=1)
`INP, OUT, SKI, SKO, ION, IOF`

> ⚠️ Minor variations exist between editions/courses—adjust to your syllabus as needed.

---

## 🚀 Getting Started
1. **Install Logisim Evolution** (tested with recent versions).
2. Open `MorisManoCPU.circ`.
3. Load `ROM.hex` via **Memory → Load Image** on the RAM component.
4. Press **Tick** (or use the clock) to step through `T0…T6`.
5. Observe register transitions via labeled probes.

### Controls & Tips
- Use **Single Step** to watch micro-ops cleanly.
- Toggle the **Run** clock for continuous execution.
- Keep `Sequence Counter` visible to correlate with the micro-op phase.

---

## 🧪 Example Program (ROM.hex snippet)
```hex
# Load value and add to AC (illustrative only)
# Address : Word
000: 200     # LDA 0x000
001: 100     # ADD 0x000
002: 700     # HLT
000: 00A     # Data at 0x000 = 10
```
> Replace with your own course/assignment ROM contents as needed.

---

## 🖼 Suggested Diagram Layout
```
[ Input/Output ]   [ Memory ]
        │              │
        ├──────┐  ┌────┤
               ▼  ▼
            [ Control ]───[ Sequence Counter T0–T6 ]
                 │
       ┌─────────┴──────────────────────────────┐
       ▼                                        ▼
   [ Registers ]                            [  ALU  ]
 AC DR IR TR PC AR OUTR INPR SC         ADD, AND, INC, SHL/SHR
```

---

## ✅ Checkpoints
- [ ] Fetch cycle toggles as expected (T0–T2)
- [ ] Decode routes to correct control signals
- [ ] Register probes show correct values
- [ ] Example program halts with expected AC value

---

## 🙏 Acknowledgments
Inspired by **Morris Mano** and the many teaching labs that brought the Basic Computer to life. Built for learning with Logisim Evolution.

---

## 📄 License
Released under the **MIT License**. Use, modify, and learn freely.
