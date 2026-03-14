# TEST PROCEDURE DOCUMENT (TPD)
## Subsystem: Line‑Drawing Engine (s8, s9, s7)
## Version: 1.0

### 1. Purpose

	This Test Procedure Document defines the exact steps, inputs, expected outputs, 
	and pass/fail criteria for verifying the Version 1.0 Line‑Drawing Engine.

	This TPD implements the Test Plan and satisfies the Verification Matrix.

### 2. Test Environment

### 2.1 Hardware / Emulator

	16‑bit x86 emulator or equivalent

	Memory‑mapped framebuffer (ES segment)

	Ability to inspect registers and memory after each call

### 2.2 Software

	Test harness capable of:

-	Setting global variables

-	Capturing ES memory writes

-	Stubbing calculate_offset()

-	Logging register changes

### 2.3 Initialization

	Before each test:

	Code

-	Clear framebuffer to 0x00
-	Clear all global variables (d06010–d063A2)
-	Initialize ES to framebuffer base
-	Reset SI, DI, AX, BX, CX, DX to 0

### 3. Test Procedures

	Each procedure includes:

-	Objective

-	Initial Conditions

-	Execution Steps

-	Expected Results

	Pass/Fail Criteria

**⭐ TP‑1: Mode Skip (FR‑1)**

	Objective

	Verify that s8() exits immediately when d063a2 == 0x02.

	Initial Conditions

	Code

	d063a2 = 0x02
	All coordinates = known values
	Framebuffer = 0x00
	Steps
	Call s8().

	Record whether s9() was invoked.

	Inspect framebuffer.

	Expected Results
	No calls to s9().

	No framebuffer writes.

	No coordinate changes.

P	ass/Fail Criteria
	PASS if all expected results match.

**⭐ TP‑2: Fast‑Path Mode (FR‑1)**

	Objective
	Verify that s8() calls b067e1() when d063a2 == 0x00.

	Initial Conditions
	Code
	d063a2 = 0x00
	Coordinates = known values
	Steps
	Call s8().

	Log whether b067e1() was invoked.

	Confirm no calls to s9().

	Expected Results
	b067e1() called exactly once.

	No multi‑pass behavior.

	Pass/Fail Criteria
	PASS if fast‑path is taken and no passes of s9() occur.

**⭐ TP‑3: Multi‑Pass Execution (FR‑2)**

	Objective
	Verify that s8() executes four passes of s9() with correct coordinate permutations.

	Initial Conditions
	Code
	d063a2 = 0x01
	d06035 = X1
	d06037 = Y1
	d06039 = X2
	d0603B = Y2
	Steps
	Call s8().

	Log each call to s9().

	After each pass, record coordinate registers.

	Expected Results
	Pass 1:

	Code
	d06039 = X1

	Pass 2:

	Code
	d06035 = X2
	d06039 = X2

	Pass 3:

	Code
	d06035 = X1
	d06037 = Y2
	d06039 = X2

	Pass 4:

	Code
	d06037 = Y1
	d0603B = Y1

	Pass/Fail Criteria
	PASS if all four passes occur and coordinate rewrites match exactly.

⭐ TP‑4: Delta Computation (FR‑3)

	Objective
	Verify correct computation of dx and dy.

	Initial Conditions
	Code
	d06035 = 10
	d06037 = 20
	d06039 = 30
	d0603B = 50
	Steps
	Call s9().

	Capture computed dx and dy.

	Expected Results
	Code
	dx = 50 - 20 = 30
	dy = 30 - 10 = 20

	Pass/Fail Criteria
	PASS if dx and dy match expected values.

**⭐ TP‑5: Bresenham Initialization (FR‑4)**

	Objective
	Verify correct initialization of Bresenham state variables.

	Initial Conditions
	Coordinates set to known values.

	Steps
	Call s9().

	Inspect:

	d06020

	d06022

	d06024

	d06026

	d06028

	d0602A

	d0602C

	d0602E

	bx (error term)

	Expected Results
	All values match Bresenham algorithm for given dx/dy.

	Pass/Fail Criteria
	PASS if all variables match expected Bresenham setup.

**⭐ TP‑6: Pixel Iteration Count (FR‑5)**

	Objective
	Verify that s7() is called exactly d06024 times.

	Initial Conditions
	Code
	d06024 = 12
	Steps
	Call s9().

	Count calls to s7().

	Expected Results
	s7() called exactly 12 times.

	Pass/Fail Criteria
	PASS if iteration count matches.

**⭐ TP‑7: Error‑Driven Step Selection (FR‑6)**

	Objective
	Verify correct branch selection based on sign of bx.

	Initial Conditions
	Run two subtests:

	A. Negative branch

	Code
	bx = -1
	B. Positive branch

	Code
	bx = 1
	Steps
	Call s9() for each subtest.

	Log updates to si, di, bx.

	Expected Results
	Negative branch:

	Code
	si += d06028
	di += d0602A
	bx += d0602C
	Positive branch:

	Code
	si += d06022
	di += d06020
	bx += d0602E
	Pass/Fail Criteria
	PASS if correct branch taken in both subtests.

**⭐ TP‑8: Pixel Plotting Logic (FR‑7)**

	Objective
	Verify mask + merge behavior in s7().

	Initial Conditions

	Code
	si = 5
	di = 10
	AH = 0xA5
	es[offset] = 0xF0
	d06010[1] = 0x0F   // because si & 3 = 1
	Steps
	Stub calculate_offset() to return known offset.

	Call s7().

	Inspect ES[offset].

	Expected Results
	Code
	new = (0xF0 & 0x0F) | 0xA5
	Pass/Fail Criteria
	PASS if ES[offset] equals computed new value.

**⭐ TP‑9: Framebuffer Integrity (FR‑8)**

	Objective
	Verify that s7() writes exactly one byte and does not modify adjacent memory.

	Initial Conditions
	Code
	Framebuffer initialized to 0x00
	Steps
	Call s7() 10 times with varying coordinates.

	Inspect entire framebuffer.

	Expected Results
	Only the expected offsets are modified.

	No adjacent bytes changed.

	Pass/Fail Criteria
	PASS if all writes are isolated and correct.

### 4. Pass/Fail Summary Sheet

	Test Case	Result	Notes
	TP‑1	□ PASS / □ FAIL	
	TP‑2	□ PASS / □ FAIL	
	TP‑3	□ PASS / □ FAIL	
	TP‑4	□ PASS / □ FAIL	
	TP‑5	□ PASS / □ FAIL	
	TP‑6	□ PASS / □ FAIL	
	TP‑7	□ PASS / □ FAIL	
	TP‑8	□ PASS / □ FAIL	
	TP‑9	□ PASS / □ FAIL
	
### 5. QA Sign‑Off

	QA Engineer: __________________________
	Date: _________________________________

	Engineering Lead: _____________________
	Date: _________________________________
