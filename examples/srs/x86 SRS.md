# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## Subsystem: Line‑Drawing Engine (s8, s9, s7)
## Version: 1.0 (Pseudo‑Code Reconstruction)

### 1. Purpose

	The Line‑Drawing Engine shall compute and render line segments into a 
	memory‑mapped framebuffer using a multi‑pass, Bresenham‑like rasterization 
	algorithm.

### 2. Functional Requirements

	- FR‑1 — Mode‑Based Dispatch

		The system shall select the line‑drawing path based on the value of 
		d063a2:

		If d063a2 == 0x02, no drawing shall occur.

		If d063a2 == 0x00, the system shall execute the b067e1 fast‑path.

		Otherwise, the system shall execute four sequential passes of s9().

	- FR‑2 — Multi‑Pass Line Rendering
		The system shall perform up to four drawing passes, each using 
		different coordinate permutations derived from:

		d06035, d06037, d06039, d0603b

	- FR‑3 — Delta Computation

		The system shall compute:

		dx = d0603b - d06037

		dy = d06039 - d06035

	- FR‑4 — Bresenham Initialization

		The system shall initialize:

		Step increments (d06020–d0602e)

		Error accumulator (bx)

		Loop count (d06024)

	- FR‑5 — Pixel Iteration

		The system shall iterate exactly d06024 times, calling s7() once per 
		iteration.

	- FR‑6 — Error‑Driven Step Selection

		The system shall update (si, di, bx) using one of two paths:

		If bx < 0: use increments (d06028, d0602a, d0602c)

		Else: use increments (d06022, d06020, d0602e)

	- FR‑7 — Pixel Plotting

		The system shall compute a framebuffer offset from (di, si) and write 
		a masked pixel value:

		Mask = d06010[si & 0x03]

		New pixel = (old & mask) | AH

	- FR‑8 — Framebuffer Integrity

		The system shall write only one byte per pixel and shall not modify 
		memory outside the ES segment.
