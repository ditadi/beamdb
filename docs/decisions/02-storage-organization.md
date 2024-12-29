# Storage Organization Decision

## Status
In Analysis

## Context
BeamDB needs to determine the fundamental approach for organizing table storage. This decision impacts:
- Access patterns efficiency
- Write performance
- Space utilization
- Implementation complexity
- Maintenance overhead

## Options Considered

### Option 1: Tuple-oriented Storage
- Traditional heap-file organization
- Unordered collection of pages with tuple data
- Uses page directory for location tracking
#### Pros
- Simple and efficient implementation
- Good for random access patterns
- Excellent for small transactions
- Low overhead for updates
- Flexible space management
#### Cons
- May need periodic reorganization
- No inherent ordering benefit
- Potential fragmentation over time

### Option 2: Log-structured Storage
- Append-only log of changes
- Uses LSM trees or similar structures
#### Pros
- Excellent write performance
- Good space utilization
- Natural support for MVCC
#### Cons
- Complex read patterns
- Requires background compaction
- Higher implementation complexity
- Read amplification
- Not optimal for random access

### Option 3: Index-organized Storage
- Table data stored within index structure
- Usually implemented with B+Tree
#### Pros
- Natural ordering
- Good range queries
- No separate index needed for primary key
#### Cons
- More complex updates
- Space overhead
- Not optimal for our OLTP focus
- More expensive maintenance

## Decision
BeamDB will implement Tuple-oriented Storage as its primary storage organization.

### Rationale
1. **Alignment with Goals**
  - Perfect for high-speed OLTP workload
  - Supports efficient random access
  - Minimal overhead for small transactions
  - Simpler implementation matches speed-first focus

2. **Operational Benefits**
  - Lower maintenance overhead
  - Simpler recovery mechanisms
  - Better space utilization for our use case
  - More predictable performance

3. **Technical Advantages**
  - Easier concurrency control
  - Simpler buffer pool management
  - More straightforward implementation
  - Better support for our transaction patterns

## Implementation Notes
1. Key Focus Areas:
  - Efficient page directory implementation
  - Smart space management
  - Optimized page allocation
  - Efficient free space tracking

2. Considerations:
  - Page size optimization
  - Directory structure design
  - Free space management strategy
  - Concurrent access patterns

## Related Decisions
- Storage Model (NSM)
- Page Layout
- Buffer Pool Design
- Index Strategy

## Follow-up
- Monitor fragmentation levels
- Track space utilization
- Evaluate need for background optimization
- Implement Storage Organization Strategy Decision
