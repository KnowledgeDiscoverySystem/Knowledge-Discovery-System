# 3,883-Page DFD -- ARM64 Assembly Reconstruction

*(This folder keeps its original "3880 Page DFD ARM64 ASM" path for link stability across releases -- the model itself now spans 3,883 pages; see below.)*

This folder contains a complete Data Flow Diagram reconstructed by KDS from a single ARM64 assembly codebase, published one page per file (`Page_1.pdf` through `Page_3883.pdf`).

## Why this update matters

This release fixes a defect in the reconstruction engine's own backtracking logic, not a problem with this source binary. A constant meant only to bound how many times a single lane-routing attempt could retry was, in three separate places, also being reused as a hard ceiling on how many times an edge could contest a blocking reservation during backtracking -- even though the engine maintains a much larger, page-wide backtracking budget specifically to prevent runaway contention, and that real budget was barely touched. Direct log evidence from this model's single highest-degree hub (267 connections) showed two edges hitting the reused ceiling at exactly 8 attempts while the real budget still had over 2,100 of its roughly 2,341 units unused -- the reused ceiling, not a lack of room, is why five real connections on that hub were being left undrawn instead of resolved. The fix removes the reused limit from all three places it had leaked into backtracking eligibility, leaving the engine's own pre-existing page-wide budget as the only ceiling -- the same category of fix its candidate-search logic received earlier this release cycle, now applied consistently across the full backtracking path.

Every page this defect could plausibly have touched was recomputed and re-validated against the complete model, not spot-checked. One further unresolved edge turned up elsewhere in the model during that validation; it was root-caused against the same evidentiary standard -- full backtrack and reservation history, not inference -- and confirmed to be the engine's own safety net correctly refusing to draw a line that would collinear-overlap another edge's already-reserved route in a genuinely overcrowded corner of the model. That is not a defect: KDS's deterministic engine does not draw a connection it cannot prove is correct, even when leaving it off the page is the harder answer, and this release's review confirms that discipline held.

## Statistics

- **Source**: 4,969,515 lines of ARM64 assembly

- **Entities extracted**: 112,480

- **Hierarchy depth**: 3,553 levels (deepest node: `Lline_table_start0_AdminAppController`)

- **Pages**: 3,883

- **System complexity**: 4,852 total (average node complexity 3.12 across 1,006 top-level entities; max node complexity 30)

- Generated with Case Structural Determination Tool v4.0.0 and Case Migration v4.0.0

## Navigating this folder on GitHub

GitHub's folder browser only lists the first ~1,000 files in a directory this large, so most pages won't appear in the file listing shown on the repository page. This does not mean they are missing -- every one of the 3,883 pages is committed to this repository. To reach a specific page directly, use its file name in the URL, for example:

```
https://github.com/KnowledgeDiscoverySystem/Knowledge-Discovery-System/blob/main/3880%20Page%20DFD%20ARM64%20ASM/Page_1500.pdf
```

Each page also carries internal navigation links to related pages, generated as part of the reconstruction.
