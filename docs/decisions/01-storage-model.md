# Storage Model Decision

## Status
In Analysis

## Context
BeamDB requires a storage model that aligns with is core mission of being a high-speed OLTP database optimized for high volumes of small transactions.
The storage model choice is fundamental as it affects:
- Transaction processing speed
- Memory Efficiency
- I/O Patterns
- Overall System Performance

## Options Considered

### Option 1: NSM (N-ary Storage Model)
- Row-oriented storage where all attributes of a tuple are stored contiguously
- Most common approach for OLTP databases
#### Pros
- Efficient for small, complete-row transactions
- Better cache locality for transaction processing
- Faster inserts and updates due to contiguous storage
- Simpler implementation and concurrency control
#### Cons
- Less efficient for column-only operations
- Potential waste when reading unused columns
- Less compressible than columnar storage

### Option 2: DSM (Decomposition Storage Model)
- Column-oriented storage where each attribute is stored separately
#### Pros
- Efficient for analytical queries
- Better compression ratios
- Good for column-based operations
#### Cons
- Poor performance for OLTP workloads
- Complex tuple reconstruction
- Higher overhead for small transactions
- Not aligned with BeamDB's goals

### Option 3: PAX (Partition Attributes Across)
- Hybrid approach combining aspects of NSM and DSM
#### Pros
- Balance between row and column access
- Improved cache performance
#### Cons
- Added complexity
- Overhead not justified for pure OLTP workload
- Implementation complexity

## Decision
BeamDB will implement the N-ary Storage Model (NSM) as its primary storage architecture.

### Rationale
1. **Workload Alignment**
  - Perfect match for OLTP transactions
  - Optimized for complete row access
  - Supports high-speed transaction processing

2. **Performance Benefits**
  - Minimizes I/O operations
  - Better cache utilization
  - Faster transaction processing
  - Simplified buffer pool management

3. **Implementation Advantages**
  - Clean and straightforward implementation
  - Better support for indexes
  - Lower system complexity
  - More efficient concurrency control

## Implementation Notes
1. Focus Areas:
  - Optimize page layout for transaction patterns
  - Implement efficient buffer pool
  - Fine-tune page sizes for optimal performance
  - Consider NUMA-aware memory allocation

2. Key Considerations:
  - Page size optimization
  - Memory alignment
  - Cache-line optimization
  - Efficient space management

## Related Decisions
- Page Layout Design
- Buffer Pool Implementation
- Compression Strategy

## Follow-up
- Monitor performance metrics
- Review decision if workload patterns change
- Consider hybrid approaches if needed for specific use cases
