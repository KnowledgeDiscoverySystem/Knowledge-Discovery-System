# TEST PLAN — Version 1.0
## Subsystem: Line‑Drawing Engine (s8, s9, s7)

### 1. Test Plan Overview

### 1.1 Purpose

	This Test Plan defines the verification approach for the Version 1.0 
	Line‑Drawing Engine.

	The objective is to confirm that the subsystem satisfies all functional 
	requirements defined in the SRS and behaves as designed in the SDD.

### 1.2 Scope

	This plan covers:

	Mode dispatch logic (s8)

	Multi‑pass line rendering

	Bresenham‑style rasterization (s9)

	Pixel plotting (s7)

	Global state variable usage

	Framebuffer writes

	Out of scope:

	External routine b067e1

	External function calculate_offset()

	ES segment initialization

	Hardware‑specific timing behavior

### 2. Test Items

	The following routines are under test:

  **Routine	Description**
-	s8	Dispatcher controlling multi‑pass rendering
-	s9	Bresenham‑like rasterizer
-	s7	Pixel plotting routine

	Global variables under test:

	d063a2 (mode)

	d06035, d06037, d06039, d0603b (coordinates)

	d06020–d0602e (step increments)

	d06010[] (mask table)

	d06024 (loop count)

### 3. Features to Be Tested

   **Feature		              Requirement	Description**
   
	   Mode dispatch	        FR‑1	      Correct branching based on d063a2
     
	   Multi‑pass rendering	    FR‑2	      Four passes executed with correct coordinate
     
				permutations
     Delta computation		    FR‑3	      Correct dx/dy calculation
     
     Bresenham initialization	FR‑4	      Correct setup of step increments and error term
     
     Pixel iteration		    FR‑5	      Loop executes exactly d06024 times
     
     Error‑driven stepping	    FR‑6	      Correct branch based on sign of bx
     
     Pixel plotting		        FR‑7	      Correct mask + merge behavior
     
     Framebuffer integrity	    FR‑8	      Writes only to intended byte
     

### 4. Features Not to Be Tested

- Performance characteristics

- Rendering quality beyond correctness

- Hardware‑specific timing

- ES segment initialization

	Behavior of external routine b067e1

### 5. Test Environment

### 5.1 Hardware / Emulator

	16‑bit x86 emulator or equivalent test harness

	Memory‑mapped framebuffer emulation

	ES segment pointing to framebuffer

### 5.2 Software

	Test harness capable of:

	Setting global variables

	Capturing memory/register state

	Stubbing calculate_offset()

	Logging all writes to ES segment

### 5.3 Test Data

	Known coordinate sets

	Known mask table values

	Known framebuffer initial states

### 6. Test Strategy

### 6.1 Unit Testing

	s8, s9, and s7 tested independently

	Global variables initialized per test case

	Framebuffer writes validated byte‑by‑byte

### 6.2 Integration Testing

	Full execution path: s8 → s9 → s7

	Multi‑pass behavior validated

	Coordinate transformations validated

### 6.3 White‑Box Testing

	Internal state variables (bx, si, di) inspected

	Step increments validated

### 6.4 Black‑Box Testing

	Pixel output validated against expected framebuffer patterns

### 7. Test Cases

	Below is the complete test suite.

**TC‑1 — Mode Dispatch: Skip Mode (FR‑1)**
	Initial State:  
	d063a2 = 0x02

	Procedure:  
	Call s8().

	Expected Result:

	No calls to s9()

	No framebuffer writes

	No coordinate changes

**TC‑2 — Mode Dispatch: Fast‑Path (FR‑1)**
	Initial State:  
	d063a2 = 0x00

	Procedure:  
	Call s8().

	Expected Result:

	b067e1() invoked

	No multi‑pass behavior

**TC‑3 — Multi‑Pass Execution (FR‑2)**
	Initial State:  
	d063a2 = 0x01  
	Coordinates set to known values.

	Procedure:  
	Call s8().

	Expected Result:  
	s9() is called exactly four times, with coordinate registers updated as follows:

**Pass 1:** d06039 = ax

**Pass 2:** d06035 = cx, d06039 = cx

**Pass 3:** d06035 = ax, d06037 = dx, d06039 = cx

**Pass 4:** d06037 = bx, d0603b = bx

**TC‑4 — Delta Computation (FR‑3)**
	Initial State:  
	Set known coordinates.

	Procedure:  
	Call s9().

	Expected Result:  
	dx = d0603b - d06037  
	dy = d06039 - d06035

**TC‑5 — Bresenham Initialization (FR‑4)**
	Procedure:  
	Call s9().

	Expected Result:

	d06020–d0602e contain valid step increments

	bx initialized to correct error term

	d06024 contains correct iteration count

**TC‑6 — Pixel Iteration Count (FR‑5)**
	Initial State:  
	d06024 = N

	Procedure:  
	Call s9().

	Expected Result:  
	s7() is called exactly N times.

**TC‑7 — Error‑Driven Step Selection (FR‑6)**
	Initial State:  
	Set bx negative, then positive in separate runs.

	Procedure:  
	Call s9().

	Expected Result:

	If bx < 0: (si, di, bx) updated using negative‑branch increments

	Else: updated using positive‑branch increments

**TC‑8 — Pixel Plotting (FR‑7)**
	Initial State:  
	Known values for si, di, AH, and es[offset].

	Procedure:  
	Call s7().

	Expected Result:  
	es[offset] = (old & mask) | AH

**TC‑9 — Framebuffer Integrity (FR‑8)**
	Procedure:  
	Call s7() repeatedly.

	Expected Result:

	Only one byte modified per call

	No writes outside framebuffer bounds

### 8. Entry Criteria

	SRS and SDD baselined

	Pseudo code validated

	Test harness operational

	Framebuffer emulator initialized

	All global variables defined

### 9. Exit Criteria

	All test cases executed

	All expected results observed

	All deviations analyzed and dispositioned

	Test Report completed

### 10. Deliverables

	Test Plan (this document)

	Test Procedures

	Test Logs

	Test Report

	Coverage Matrix

	QA Sign‑Off
