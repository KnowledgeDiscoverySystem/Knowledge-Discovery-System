# 3,883-Page DFD -- ARM64 Assembly Reconstruction

*(This folder keeps its original "3880 Page DFD ARM64 ASM" path for link stability across releases -- the model itself now spans 3,883 pages; see below.)*

This folder contains a complete Data Flow Diagram reconstructed by KDS from a single ARM64 assembly codebase, published one page per file (`Page_1.pdf` through `Page_3883.pdf`).

## Why this update matters

This entire model -- all 3,883 pages of it -- was found to need correction, fixed, rebuilt, and republished in under a single day. Real defects surfaced in review (a dataflow's label overwriting the very process or terminal it describes, an off-page reference column crowding its own connector line, a macOS export dialog silently failing to appear) went from "spotted on screen" to "fixed in the underlying tool, verified against this full 5-million-line model, and live on GitHub" in the same working session. That turnaround -- diagnose, fix, regenerate a model of this size, and validate the result -- is not the pace legacy-system reverse engineering has run at before. A CASE tool that can roll a lesson learned into the next full model same-day, rather than next release cycle, changes what "iterating on a model" means: review stops being a one-way report and becomes a loop you can close before the day is out.

This release also completes the model's reference resolution against the target's own compiler/runtime library. Every call this codebase makes into the Qt framework it links against -- `QApplication`, `QCoreApplication`, `QWidget`, `QJsonValue`, and the rest -- is now identified and drawn as its own entity, standing alongside the application's own logic in the same DFD, rather than showing up as an unresolved or opaque external call. The picture this model draws is no longer just "your code" -- it's your code and everything it actually runs on, as one connected system.

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
