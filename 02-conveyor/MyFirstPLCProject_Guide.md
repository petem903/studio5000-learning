# My First PLC Project â€” Quick Start Guide

> **File**: `MyFirstPLCProject.L5X` â€” open this in Studio 5000 first.
> **What it is**: A conveyor that starts, stops, counts boxes, and stops automatically after N boxes.
> **8 routines**, each teaching ONE new concept. Start at Routine 01.

---

## How to Open the Project

1. Launch **Studio 5000 Logix Designer**
2. **File â†’ Open** â†’ browse to `MyFirstPLCProject.L5X`
3. The project opens. You do NOT need a physical PLC.
4. On the left, expand **Tasks â†’ MainTask â†’ MainProgram**
5. Double-click **Routine_00_MainOrchestrator** to see the table of contents

---

## The 8 Routines (Study in Order)

| # | Routine Name | What You Learn | # of Rungs |
|---|---|---|---|
| 00 | MainOrchestrator | JSR â€” calling subroutines | 7 |
| **01** | **YourFirstRung** | **XIC and OTE â€” your first rung ever** | **1** |
| **02** | **SealIn_StartStop** | **The seal-in circuit â€” press once, runs forever** | **1** |
| 03 | AddSafety_EmergencyStop | XIO for E-Stop, series contacts = AND | 1 |
| 04 | AddTimer_StartDelay | TON â€” wait 3 seconds before starting | 2 |
| 05 | CountParts | CTU + ONS â€” count each box exactly once | 2 |
| 06 | Decision_AutoStop | GEQ â€” stop automatically after N boxes | 2 |
| 07 | StatusLights | Green/Red/Amber beacons, multiple outputs | 4 |

**The first three routines (01-03) are the most important.** If you understand those, you understand 80% of what PLCs do in the real world.

---

## What Each Tag Represents

All tags use plain English names. No abbreviations.

| Tag Name | What It Is |
|---|---|
| `PushButton_Green_Start` | The green Start button (momentary) |
| `PushButton_Red_Stop` | The red Stop button (momentary, NC) |
| `PushButton_Mushroom_EmergencyStop` | The big red E-Stop mushroom (NC) |
| `PushButton_Blue_Reset` | Blue reset button (momentary) |
| `Sensor_PhotoEye_PartDetected` | Photo-eye that sees boxes on the conveyor |
| `Sensor_Gate_SafetyDoorClosed` | Safety gate limit switch |
| `Motor_Conveyor_RunCommand` | The output that makes the conveyor move |
| `Light_Green_MachineRunning` | Green beacon â€” machine is running |
| `Light_Red_MachineStopped` | Red beacon â€” machine is stopped |
| `Light_Amber_BatchComplete` | Amber beacon â€” batch is done |
| `Timer_StartDelay` | 3-second delay timer (TON) |
| `Counter_TotalPartsProduced` | How many boxes have passed (CTU) |
| `Tag_HowManyPartsBeforeStopping` | Batch size â€” stop after this many boxes |
| `Bit_SealIn_ConveyorRunning` | Internal memory â€” "should the conveyor be on?" |
| `Bit_OneShot_PartEdge` | One-shot storage â€” don't count the same box twice |
| `Bit_BatchComplete` | Internal flag â€” "we made enough boxes" |

---

## The Three Instructions You MUST Know

### XIC â€” "Examine If Closed" (the `â”€] [â”€` contact)

```
XIC(PushButton_Green_Start)
```
- TRUE when the tag = 1 (button IS pressed)
- FALSE when the tag = 0 (button is NOT pressed)
- Think: "Is this thing ON?"

### XIO â€” "Examine If Open" (the `â”€]/[â”€` contact)

```
XIO(PushButton_Mushroom_EmergencyStop)
```
- TRUE when the tag = 0 (E-Stop is NOT pressed)
- FALSE when the tag = 1 (E-Stop IS pressed)
- Think: "Is this thing NOT tripped?"

### OTE â€” "Output Energize" (the `â”€( )â”€` coil)

```
OTE(Motor_Conveyor_RunCommand)
```
- Makes the tag = 1 when power reaches the coil
- Makes the tag = 0 when power does NOT reach the coil
- Think: "Set this tag to match whatever the rung says"

---

## How to Read a Rung (Left â†’ Right, like English)

```
[XIC(Button)]â”€â”€â”€[XIO(Stop)]â”€â”€â”€(OTE(Motor))
   "If button     "AND stop      "THEN turn on
    is pressed"    is NOT hit"    the motor"
```

That's it. Contacts on the left are conditions. The coil on the right is the result. Read left to right. If all conditions are TRUE, the coil energizes.

**Series contacts = AND**: ALL must be TRUE.
```
[A]â”€â”€â”€[B]â”€â”€â”€[C]â”€â”€â”€(Output)    means: A AND B AND C
```

**Parallel branches = OR**: AT LEAST ONE must be TRUE.
```
   â”Œâ”€â”€[A]â”€â”€â”
â”€â”€â”€â”¤       â”œâ”€â”€â”€(Output)        means: A OR B
   â””â”€â”€[B]â”€â”€â”˜
```

---

## The Seal-In Circuit (Routine 02 â€” Most Important Pattern)

```
   â”Œâ”€â”€[Start Button]â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”€â”€â”€â”¤                          â”œâ”€â”€â”€[Stop Button]â”€â”€â”€(Memory Bit)
   â””â”€â”€[Memory Bit (seal)]â”€â”€â”€â”€â”€â”˜
```

1. Press Start â†’ memory bit turns ON
2. The lower branch "seals" â€” keeps memory ON after you let go of Start
3. Press Stop â†’ memory bit turns OFF
4. The seal breaks â€” stays OFF

Every motor, pump, and conveyor in a factory uses this pattern.

---

## Walkthrough: Creating This from Scratch

### Step 1: New Project
File â†’ New â†’ pick 1756-L72 â†’ name it â†’ Finish.

### Step 2: Create the Tags
Double-click **Controller Tags** in the tree. Click the **Edit Tags** tab at the bottom. Type each tag name, pick its Data Type from the dropdown. BOOL for on/off things, DINT for numbers, TIMER and COUNTER for their respective types.

### Step 3: Add Routines
Right-click **MainProgram** â†’ **Add â†’ New Routine**. Name it, choose Type: Ladder Diagram. Repeat for all 8 routines. Set MainRoutineName (in Program Properties) to `Routine_00_MainOrchestrator`.

### Step 4: Write Rungs
Open a routine. Click the **Add Rung** button (or right-click in the editor â†’ Add Rung). Drag instructions from the toolbar onto the rung, or type the mnemonic (XIC, OTE, etc.) and press Enter. Double-click the tag field and type the tag name.

### Step 5: Verify
Click the **Verify** button (checkmark icon) or press Ctrl+Shift+F7. The Results window shows any errors. Fix them and verify again until clean.

---

## Try These Modifications

1. **Change the batch size**: Edit `Tag_HowManyPartsBeforeStopping` from 10 to 5. Now the conveyor stops after 5 boxes.
2. **Change the start delay**: In Routine 04, change TON preset from 3000 to 5000 (5 seconds).
3. **Add a second conveyor**: Create a copy of all the tags with "Conveyor2" in the name and a copy of the logic.
4. **Add a flashing amber light**: Use a self-resetting timer pattern (XIO of .DN â†’ TON) to flash the amber beacon.

---

## When You're Ready for More

After you understand all 8 routines, move on to:
- `Beginner_Guide_Annotated.L5X` â€” adds math, comparisons, trends, periodic tasks, UDTs, AOIs
- `Beginner_Study_Guide.md` â€” detailed concept reference

But don't rush. Spend time with this simple conveyor project first. Change things. Break things. Fix them. That's how you learn.
