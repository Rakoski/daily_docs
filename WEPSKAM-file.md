# Notes on What Every Programmer Should Know About Memory

## Section 2 - Commodity hardware today

### There are reasons why not all memory is SRAM

### Memory cells need to be individually selected to be used
- The number of address lines is directly responsible for the cost of the memory controller, motherboards, DRAM module, and DRAM chip
- It takes a while before the results of the read or write operation are available

### The more GHz the better for DRAM
- The more the array freq. the more the bus freq. the more the data rate and the higher the speed will be
- Array freq of 200mhz -> bus freq 800mhz -> data rate 12.800mb/s -> name of the rate PC3-12800 -> name (fsb) DDR-1600
- The sooner the precharging can happen and the Row access section signal is sent the smaller the penalty will be when the row is used

## Section 3 - CPU Caches

### RAM as caches is SRAM
### RAM as main memory is DRAM
### THE CPU CAN ONLY PERFORM DATA OPS IN L1D

### Policies (for my brain memory self refreshment)
Hits:
- Write through: every write goes to cache and then main memory
- Write back: writes to cache and mark as dirty, then, when line gets evicted (LRU), writes to main memory
  
Misses:
- Write allocate: miss -> writes to memory and then cache, then acts as if it was a hit
- No write allocate: miss -> just writes to main memory, don't bother with cache at all

Every evicted line gets marked as invalid. invalid lines always result in misses

### All loads and stores have to go through the cache
  
- The connection between the cache and main memory is the system bus which also connects to other components
- A modern processor with two cores have individual threads that shares the level 1 cache each (1 processor, 2 cores, 4 threads, 2 level 1 cache (per core)). The processors do not share any caches
- A dirty cache line is not present in any other processor's cache
- Clean copies of the same cache line can reside in arbitrarily many caches.
- Larger cache sizes generally mean faster programs. Sequential access is also prefered in large caches
- Virtual addresses can over time refer to different physical addresses, thus the memory management unit (MMU) has to translate them (the time of the operation depends on the size of the memory unit, so it is almost always beneficial to do them in the lower level caches than in the actual main memory)
- Use logical memory completely or use page sizes as large as meaningful to diversify the physical addresses as much as possible
- Front-side bus: data bus (physical electrical connections): was the data bus between the CPU and the memory controller -> doesn't exist anymore. Instead, we use direct connections and higher bandwidth such as QuickPath Interconnect
- Write through also means that for write access and copy access are limited by L2 cache speed
- The more cores, the bigger the delay for write and copy accesses
- If there are two processors and a program is running two threads, if the program is still in L1, they compete for the same memory location and the resulting performance is not great as it is the same as in the main memory since they are mostly comprised by misses all the way to the main memory. They only get better once L1 is not sufficient, by which L2 takes the hold and increases performance and RFO (main memory messages) are needed when data has not yet been flushed (writing back to main memory)

## Section 4 - Virtual Memory

### Virtual memory allocates data in branches of levels of virtual addresses, so while a pdf reader (small process) might just have a root on a level 4 directory and maybe 3 branches (One branch for its code, One branch for its heap (where it loads the PDF) and one for the stack) going all the way to the physical page, the JVM process has a root on the level 4 directory and say 100 branches all going all the way to the physical page, along with that, a jvm process can map to say 10000 pages, while a PDF reader's largest might just map to say, 100 pages at most
- Levels: kinda like a library -> separate the address (say 0xFF432423942) into 5, level 4, 3, 2, 1, and physical (offset).
- Works kinda like if you have a book you need to find in a library -> level 4 would be the location of the library in the city, 3 would be which library floor, 2 which room, 1 which shelf, and the offset/physical what part of the shelf
  
### TLB: basically a cache of all the addresses that have been most recently used. Modern ones provide multi-level TLB caches and is also set-associative (matrices were used to calculate the sets and groups for each line of cache represented in the actual address in the cache simulator we made)
- TLB flushes happen when the page table tree changes (context switches -> CPU was into one process then goes into another -> was playing a game then opened web browser -> context switch -> flushes TLB)
- To not flush TLB all the time we have a 1 bit Address Space ID (ASID) attached to the tag which basically says which process this translation in the TLB this belongs to -> extended tags
  
### Basically virtualization works as another "translation layer" between the virtualized OS that mimics pagination and virtual memory vs the actual physical hardware, so with say VirtualBox you actually have 4 layers instead of the normal 2 (userspace + kernel), having the guest kernel userspace + guest kernel + VMM (virtual machine monitor) or hypervisor + actual OS kernel

## Section 5 - NUMA (Non Uniform Memory Architectures) Support

- mainly used for big machines with the problem multiple cores of multiple processors accessing the same memory. numa is not meant as a commodity system
- hypercubes are pretty efficient in using nodes up to around 8 nodes (in modern times), so C = 3
- beyond that, specielized hardware is required, so usually what is formed is that there are commodity machines connected with each other using high-speed networking to form a cluster, although these are nnnot NUMA machines and do not implement a shared address space in memory
- .SO files in unix machines are Executable and Linkable Format, like DLLs (Dynamic Link Libraries) in windows and they are both DSOs (Dynamic Shared Objects) files
- the OS should not try to migrate any processes or threads between processor nodes, since this means the cache content is lost 

## Main Takeaways

### Static arrays are more efficient than dynamic arrays
### Memoize stuff on the frontend so no more requests are sent and instead keep the info in the main memory
### There are 2^S sets of cache lines
### Cloud systems use SMPs or multi processors, but with multi processors come multiple L1 and L2 and L3 caches, while all processors must see the same memory content at all times. This brings challenges
