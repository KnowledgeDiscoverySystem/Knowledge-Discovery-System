# 3880-Page DFD -- ARM64 Assembly Reconstruction

This folder contains a complete Data Flow Diagram reconstructed by KDS from a single ARM64 assembly codebase, published one page per file (`Page_1.pdf` through `Page_3883.pdf`).

*Updated 2026-09-21: re-exported after a round of diagram layout fixes (dataflow labels no longer overwrite adjacent process/term boxes, off-page/Terminator columns space themselves correctly, and PDF export dialogs on macOS were fixed) -- same source, cleaner rendering. Page count moved from 3,880 to 3,883.*

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
