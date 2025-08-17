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
```
**Each cache level is fully configurable**:
- Size, block size, associativity
- Latency (cycles)
- Replacement policy (LRU/FIFO/RANDOM)

## Usage

### Compilation

```bash
g++ -std=c++17 -O3 cache_simulator.cpp -o cache_sim
```

### Running Simulations
- **Prepare a trace file (trace.txt) with memory access patterns**:
 ```plaintext
R 0x1000  # Read from address 0x1000
W 0x2000  # Write to address 0x2000
```

- **Execute with default configuration**:
```bash
./cache_sim
```

### Custom Configuration
- **Modify main() to set up your cache hierarchy**:
```bash
CacheSimulator L1(32, 4, 4, 1);       // 32KB, 4B blocks, 4-way, 1 cycle latency
CacheSimulator L2(128, 4, 4, 10);     // 128KB, 4B blocks, 4-way, 10 cycles
CacheSimulator L3(512, 4, 4, 50);     // 512KB, 4B blocks, 4-way, 50 cycles
MainMemory DRAM;

// Connect hierarchy
L1.connectLowerLevel(&L2);
L2.connectLowerLevel(&L3); 
L3.connectMainMemory(&DRAM);

// Set policies
L1.setReplacementPolicy(CacheSimulator::ReplacementPolicy::LRU);
L1.setExclusive(false);  // Inclusive mode
```

## Key Components

### Core Classes

- **CacheSimulator**:
  - Handles cache accesses, misses, and replacements
  - Supports three replacement policies
  - Implements write-back/write-through logic

- **MainMemory**:
  - Models DRAM with read/write statistics
  - Tracks total accesses

### Advanced Features
- **Bypass mode**: Skips caching for specific address ranges
- **Exclusive mode**: Ensures data exists in only one cache level
- **Flush mechanism**: Writes back dirty lines on demand
- **Prefetching**: Supports next-line prefetching

## Output Example
```plaintext
L1: Hits: 4521, Misses: 479
L2: Hits: 312, Misses: 167  
L3: Hits: 120, Misses: 47
DRAM: Reads = 47, Writes = 89
```
## Future Enhancements
- Cycle-accurate timing model
- Visualizer for cache state transitions
- Support for more replacement policies (e.g., PLRU)


