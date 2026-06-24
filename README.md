# Studio 5000 â€” Open Source Learning Projects

> **Learn PLC programming with nothing but Studio 5000 and a mouse. No hardware. No PLC. No wires.**

This repository contains four progressively-advanced Studio 5000 projects that teach ladder logic from absolute zero. Every rung is commented. Every tag is named in plain English. Start with Project 1 and work your way up.

---

## What You Need

- **Studio 5000 Logix Designer** (v32 or newer, any paid edition â€” the free Mini edition cannot open L5X files)
- A mouse and a keyboard
- That's it. No PLC. No wires. No buttons.

---

## Projects (Start Here â†’)

### Project 1: No-Hardware Basics
**Folder**: `01-basics/`

| File | Description |
|---|---|
| `LearnPLC_NoHardware.L5X` | **Start here.** 6 routines: seal-in circuit, timers, counters, comparisons. All tags use `Sim_` prefix to make it clear these are just memory bits you toggle manually. |
| `LearnPLC_NoHardware_Guide.md` | Walkthrough showing exactly how to test each routine using the Controller Tags editor. |

**What you learn**: Bits as switches, XIC/XIO/OTE, the seal-in circuit, TON timers, CTU counters, GEQ comparisons, status lights.

### Project 2: First Conveyor Project
**Folder**: `02-conveyor/`

| File | Description |
|---|---|
| `MyFirstPLCProject.L5X` | 8 routines building a conveyor that starts, stops, counts boxes, and auto-stops. |
| `MyFirstPLCProject_Guide.md` | Step-by-step companion guide with modification exercises. |

**What you learn**: Same concepts as Project 1 with different tag names (for practice), plus JSR subroutine calls, multiple routines working together.

### Project 3: Advanced Concepts
**Folder**: `03-advanced/`

| File | Description |
|---|---|
| `Beginner_Guide.L5X` | A part inspection station covering 13 concept categories: bit logic, timers, counters, math, comparisons, data handling, program control, structured text, UDTs, AOIs, tasks, trends. |
| `Beginner_Guide_Annotated.L5X` | Same project with extensively-commented rungs and descriptive tag names. ~75 rungs across 9 routines. |

**What you learn**: ADD/SUB/MUL/DIV/CPT, EQU/NEQ/GRT/LES/GEQ/LEQ/LIM/CMP, MOV/COP/CPS/CLR, JMP/LBL/FOR/NXT/RET, Structured Text (ST) language, User-Defined Data Types (UDTs), Add-On Instructions (AOIs), Periodic tasks, Trends.

### Project 4: Study Reference
**Folder**: `04-reference/`

| File | Description |
|---|---|
| `Beginner_Study_Guide.md` | Comprehensive 900-line reference covering every instruction with examples, common mistakes, practice exercises, and a cheat sheet. |

---

## Quick Start (30 Seconds)

1. Open any `.L5X` file in Studio 5000 (**File â†’ Open**, no PLC needed)
2. Double-click **Controller Tags** on the left to open the tag editor
3. Find any `Sim_Bit_` tag, click its **Value** cell, type `1` or `0`, press Enter
4. Your mouse is the hardware. You toggle bits manually to simulate button presses.
5. Open a routine and read the rung comments â€” they explain what each rung does.

**Note**: Without a real PLC or RSLogix Emulate 5000 connected, the rungs will not execute automatically. You must manually set both input and output tag values to trace through the logic. This is still an effective way to learn â€” you are tracing the logic with your brain and your mouse, which builds deeper understanding than watching automatic execution.

---

## Learning Path

```
Project 1 (1-2 hours) â†’ Project 2 (1-2 hours) â†’ Project 3 (3-6 hours) â†’ Project 4 (reference)
   Basics & seal-in       Multiple routines,       Math, comparisons,      Keep open while
   circuit pattern        JSR, counters,          UDTs, AOIs, tasks,      working on Projects
                          timers, decisions       structured text          1-3
```

---

## Why This Exists

Rockwell Automation's Studio 5000 is the industry standard for factory automation in North America. But learning it typically requires:

- A $5,000+ PLC
- Physical buttons, sensors, and motors
- An instructor or expensive training course

This repository proves you can learn the fundamentals with **none of that**. Just the software and a willingness to read rung comments and toggle bits.

The projects simulate a simple conveyor/inspection system â€” something every manufacturing engineer understands. The comments explain not just WHAT each instruction does, but WHY you'd use it and HOW to avoid common mistakes.

---

## Contributing

Found a bug? Have an idea for a new teaching routine? Open an issue or pull request.

Areas where help is welcome:
- Additional practice exercises
- Function Block Diagram (FBD) examples
- Sequential Function Chart (SFC) examples
- Translations of comments to other languages
- A "common error" database with fixes

---

## License

This project is dedicated to the public domain under the [Creative Commons Zero (CC0)](https://creativecommons.org/publicdomain/zero/1.0/) license. You can copy, modify, distribute, and use these files for any purpose â€” including commercial training â€” without permission or attribution. Learning industrial automation should be free.
