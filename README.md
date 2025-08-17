# Cache Simulator

A configurable multi-level cache simulator written in C++ that models write-back/write-through policies, replacement algorithms, and exclusive/inclusive cache hierarchies.

## Features

- **Multi-level cache support** (L1, L2, L3, DRAM)
- **Configurable policies**:
  - Write-back vs write-through
  - Write-allocate vs no-write-allocate
  - LRU/FIFO/RANDOM replacement
  - Inclusive/exclusive hierarchy modes
- **Bypass capability** for MMIO/streaming addresses
- **Statistics tracking**: Hits, misses, memory accesses
- **Trace-driven simulation** from input files

## Architecture

```plaintext
CPU → L1 → L2 → L3 → DRAM
