# Readers_Writers_Problem
The Readers-Writers problem is a classical synchronization problem, where multiple threads are competing for access to a shared resource, with different types of access modes (Reader/Writer). In this problem there are two types of threads: readers and writers. Readers only read from the shared resource, therefore any number of readers can access at the same time. Whereas writers modify the shared resource, therefore only one writer can access at a time (no readers are allowed to read during writing). The goal is to ensure that readers and writers can access the shared resource safely and efficiently.
## Solution
The solution to the Readers-Writers problem presented here is a starve-free solution, which means that it guarantees that no thread will have to wait indefinitely. This solution is implemented using mutexes and condition variables for synchronization
5 variables have been used in the pseudocode: read_count, write_count, mutex, read_mutex, write_mutex, turn.
'read_count' keeps a track of the number of active reader processes, it is initialised to zero.
```
int read_count = 0;
```
'write_count' keeps a track of the number of active writer processes, it is initialised to zero.
```
int write_count = 0;
```
