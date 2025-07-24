# SKY130 Day 4 - Pre-layout Timing Analysis and Importance of Good Clock Tree

---

## 🧠 Concepts Covered

### 🕒 Timing Modelling Using Delay Tables

Delay tables provide the necessary delays for standard cells depending on input transition and output load. They are typically extracted via simulation across process-voltage-temperature (PVT) corners and stored in Liberty (`.lib`) files.

For **pre-layout timing**, we need to:

- Understand **delay paths** and how delays are computed before routing.
- Use **LEF/DEF** to extract track/grid info.
- Learn the **importance of a good clock tree** for timing convergence.

---

## 🔧 Lab 55 - Converting Grid Info to Track Info

This step involves translating the routing grid from physical dimensions (from LEF) to logical track placements for EDA tools to use during routing and timing estimation.

### 🔍 Image: Grid Info → Track Info

![Grid to Track Mapping](https://github.com/user-attachments/assets/e92c6d60-f378-44b1-8bfb-26654f460845)

> **Why this matters:**  
> Clock routing and timing need to be precisely aligned to physical layout constraints.  
> You can't estimate pre-route timing well unless you know where the tracks are — this is where the **LEF** comes in.

---

## 📦 What is LEF?

**LEF (Library Exchange Format)** files describe:

- Standard cell dimensions
- Pin positions
- Routing grids
- Layer info
- Spacing rules

This allows tools to:

- Do **placement** (cell alignment to rows/tracks)
- Do **routing estimation** and **timing modeling**
- Understand **blockages, vias, and wires** for congestion analysis

---

## ⏱️ Pre-Layout Timing with Delay Tables

Pre-layout timing uses:

1. **Estimated net lengths** (from floorplan)
2. **Parasitics models** (from routing layer definitions in LEF)
3. **Delay tables** from `.lib`

These three allow you to:

- Perform **early timing analysis**
- Optimize **logic depth** and **critical paths**
- Refine **clock tree synthesis (CTS)** strategies before full route

---

## 🌳 Why Clock Tree Matters

Clock skew and delay heavily influence:

- Setup/Hold violations
- Power (due to unnecessary toggling)
- Data path performance

A bad clock tree can kill a design — get it wrong here and later timing closure becomes impossible.

---

## ✅ Key Takeaways

- Use **LEF** to map tracks, pins, and layers.
- **Delay tables** power early timing modeling (via `.lib`).
- Good **clock tree design** starts in pre-layout.
- Lab 55 bridges physical LEF info with logical track/timing modeling.

---

> 🧠 _Pro Tip_: Always analyze your longest paths and shortest hold paths pre-layout. Don’t wait until route is done.

---
