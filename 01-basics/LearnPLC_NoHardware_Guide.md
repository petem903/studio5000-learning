# Learn PLC Programming â€” No Hardware Required

> **File**: `LearnPLC_NoHardware.L5X` â€” open in Studio 5000
> **What you need**: Studio 5000, a mouse, and this guide. No PLC. No wires. No buttons.

---

## The Big Idea: Everything Is Just Switches (Bits)

Forget hardware for now. The PLC's memory is just a wall of switches, each labeled with a name.

| Switch Name | What It Represents |
|---|---|
| `Sim_Bit_StartCommand` | 0 = nobody pressing Start. **1 = you "pressed" Start.** |
| `Sim_Bit_StopCommand` | 0 = nobody pressing Stop. 1 = you "pressed" Stop. |
| `Sim_Bit_MotorRun` | 0 = conveyor off. **1 = conveyor running.** |
| `Sim_Bit_RunMemory` | 0 = forget. **1 = remember that Start was pressed.** |
| `Sim_Bit_PartAtSensor` | 0 = no box. **1 = a box is in front of the sensor.** |
| `Sim_Number_BatchSize` | How many boxes before auto-stop (default 10). |

**Your job**: flip these switches with your mouse and watch other switches change in response. That's what the PLC program does â€” it's a set of rules that say "IF this switch is UP, THEN make that switch UP."

---

## How to Test: Your Mouse = The Hardware

### Step 1: Open the Tag Editor
Double-click **Controller Tags** in the tree on the left. You'll see a spreadsheet:

```
Name                          | Value | Data Type
Sim_Bit_StartCommand          | 0     | BOOL
Sim_Bit_StopCommand           | 0     | BOOL
Sim_Bit_EmergencyStopOK       | 1     | BOOL
Sim_Bit_MotorRun              | 0     | BOOL
Sim_Bit_RunMemory             | 0     | BOOL
...
```

### Step 2: Flip a Switch
Click the **Value** cell next to `Sim_Bit_StartCommand`. Type `1`. Press Enter.
You just "pressed the Start button."

### Step 3: Watch What Happens
The PLC scan runs immediately. Check other tags:
- Did `Sim_Bit_RunMemory` change to 1? (the seal-in engaged)
- Did `Sim_Bit_MotorRun` change to 1? (the motor should be running)

### Step 4: Flip It Back
Type `0` in `Sim_Bit_StartCommand`. Press Enter. You "released" Start.
- `Sim_Bit_RunMemory` should STILL be 1 (the seal-in remembers!)

### Step 5: Press Stop
Type `1` in `Sim_Bit_StopCommand`. Press Enter.
- `Sim_Bit_RunMemory` should go to 0 (seal broken)
- `Sim_Bit_MotorRun` should go to 0 (motor stopped)

**That's it.** You just tested the seal-in circuit with no hardware. Repeat this pattern for every routine: flip input switches, watch output switches.

---

## Routine Walkthrough (Test Each One)

### Routine 00 â€” READ ME FIRST
Open this routine. Read the rung comment. It's pure explanation â€” no ladder logic. It teaches what bits are and how the scan cycle works.

### Routine 01 â€” How Bits Work (3 rungs)
**Concept**: XIC, XIO, OTE. One switch controls another.

**Test**: Toggle `Sim_Bit_StartCommand` to 1. Watch `Sim_Bit_RunMemory` follow it. Toggle it back. Read the rung comments â€” they explain XIC and XIO in plain English with truth tables.

### Routine 02 â€” Seal-In Circuit (1 rung)
**Concept**: The seal-in pattern. Parallel branches = OR. The magic "memory" rung.

**Test**:
1. `Sim_Bit_StartCommand` = 0, `Sim_Bit_RunMemory` = 0. Nothing happens.
2. Toggle `Sim_Bit_StartCommand` to 1 then 0 (quick press). `RunMemory` becomes 1 and STAYS 1.
3. Toggle `Sim_Bit_StopCommand` to 1 then 0. `RunMemory` becomes 0.

### Routine 03 â€” Series Contacts / AND Logic (2 rungs)
**Concept**: ALL conditions in series must be TRUE. Adding safety.

**Test**:
1. Start the conveyor (toggle Start 1â†’0).
2. Motor runs. Now toggle `Sim_Bit_EmergencyStopOK` to 0. Motor stops.
3. Toggle it back to 1. Motor restarts (RunMemory was never broken in this version).
4. Now toggle `Sim_Bit_GateClosed` to 0. Motor stops. Toggle back to 1. Motor restarts.

### Routine 04 â€” Timer / Start Delay (2 rungs)
**Concept**: TON â€” wait before acting. The `.DN` bit goes TRUE when time is up.

**Test**:
1. Start the conveyor. `Sim_Bit_RunMemory` = 1.
2. `Sim_Timer_StartDelay.ACC` starts counting toward 3000.
3. For quick testing: change the TON preset from 3000 to 100 (in the rung text). Now the delay is 0.1 seconds â€” effectively instant for testing.
4. `Sim_Bit_MotorRun` only turns on when timer `.DN` = 1.

### Routine 05 â€” Counter / Count Parts (2 rungs)
**Concept**: CTU counts 0â†’1 transitions. ONS prevents counting every scan.

**Test**:
1. Watch `Sim_Counter_PartsProduced.ACC` in the tag editor. It should be 0.
2. Toggle `Sim_Bit_PartAtSensor` to 1. `.ACC` becomes 1.
3. Toggle it to 0, then 1 again. `.ACC` becomes 2.
4. Toggle it to 1 without going to 0 first. `.ACC` does NOT change (ONS blocks repeat counts).
5. Toggle `Sim_Bit_Reset` to 1 then 0. Counter resets.

### Routine 06 â€” Decision / Auto-Stop (2 rungs)
**Concept**: GEQ compares two numbers. When count >= batch size, set a flag.

**Test**:
1. Set `Sim_Number_BatchSize` to 3 (small batch for quick test).
2. Start the conveyor.
3. Toggle `Sim_Bit_PartAtSensor` three times (1â†’0â†’1â†’0â†’1â†’0).
4. Counter reaches 3. `Sim_Bit_BatchDone` becomes 1.
5. Seal-in breaks. Motor stops.

### Routine 07 â€” Status Lights (4 rungs)
**Concept**: Translate internal bits into "lights." Multiple outputs.

**Test**:
1. When motor is running: `Sim_Bit_LightGreen` = 1, `Sim_Bit_LightRed` = 0.
2. Stop the motor: lights swap (Green = 0, Red = 1).
3. When batch is done: `Sim_Bit_LightAmber` = 1.

---

## The Three Instructions â€” Memorize These

```
XIC(tag)  =  â”€] [â”€  =  "Is this switch UP (1)?"
XIO(tag)  =  â”€]/[â”€  =  "Is this switch DOWN (0)?"
OTE(tag)  =  â”€( )â”€  =  "Set this switch to match the rule."
```

### XIC and XIO Truth Table

| Tag Value | XIC Result | XIO Result |
|---|---|---|
| 0 | FALSE (open) | **TRUE** (closed) |
| 1 | **TRUE** (closed) | FALSE (open) |

XIO is the "opposite day" instruction. It's TRUE when the switch is OFF.

### Series = AND, Parallel = OR

```
â”€â”€[A]â”€â”€[B]â”€â”€[C]â”€â”€     ALL must be TRUE (A AND B AND C)

â”€â”€â”¬[A]â”¬â”€â”€             AT LEAST ONE must be TRUE
  â””[B]â”˜               (A OR B)
```

---

## Quick Reference: Tag Names

All tags start with a prefix that tells you what they are:

| Prefix | Meaning | Example |
|---|---|---|
| `Sim_Bit_` | A switch (BOOL, 0 or 1) | `Sim_Bit_StartCommand` |
| `Sim_Timer_` | A timer (TON) | `Sim_Timer_StartDelay` |
| `Sim_Counter_` | A counter (CTU) | `Sim_Counter_PartsProduced` |
| `Sim_Number_` | A number (DINT) | `Sim_Number_BatchSize` |

All of these live in the Controller Tags. You change their values with your mouse. There is no hardware.

---

## What To Do Next

1. **Change numbers**: Edit `Sim_Number_BatchSize` to 5. Edit the TON preset to 500. See how the behavior changes.
2. **Break things**: Delete a contact from a rung. Verify the project (Ctrl+Shift+F7). Read the error. Fix it. This is how you learn.
3. **Add your own rung**: In Routine 07, add a rung that turns on `Sim_Bit_LightAmber` when the timer is actively counting (`.TT` bit).
4. **When comfortable**: Try `MyFirstPLCProject.L5X` (same concepts, different tag names). Then `Beginner_Guide_Annotated.L5X` (adds math, trends, UDTs, AOIs).
