# Readers_Writers_Problem
The Readers-Writers problem is a classical synchronization problem, where multiple threads are competing for access to a shared resource, with different types of access modes (Reader/Writer). In this problem there are two types of threads: readers and writers. Readers only read from the shared resource, therefore any number of readers can access at the same time. Whereas writers modify the shared resource, therefore only one writer can access at a time (no readers are allowed to read during writing). The goal is to ensure that readers and writers can access the shared resource safely and efficiently.
## Solution
The solution to the Readers-Writers problem presented here is a starve-free solution, which means that it guarantees that no thread will have to wait indefinitely. This solution is implemented using mutexes and condition variables for synchronization
4 variables have been used in the pseudocode: read_count, read_mutex, turn, resource.
1. 'read_count' keeps a track of the number of active reader processes, it is initialised to zero.
```cpp
int read_count = 0;
```
2. 'read_mutex' is a binary semaphore which protects the read_count variable.
3. 'turn' is a semaphore used at the beginning of both the reader and writer codes. Any reader/writer has to acquire this semaphore in order to enter the critical section, each having equal priority of doing so.
4. 'resource' is a semaphore which ensures that only one process can access the shared resource at a time, preventing race conditions and inconsistencies in the shared resource. It helps in ensuring mutual exclusion between readers and writers.
```cpp
semaphore read_mutex=1, turn=1, resource=1;
```
## Pseudocode
```cpp
// Initialising variables
int read_count = 0;
semaphore read_mutex=1, turn=1, resource=1;
### Reader process:
- First, a reader acquires turn, so that no other resource can acquire turn.
- Then, it locks access to the read_count variable using the read_mutex semaphore to update the number of readers currently accessing the shared resource.
- If it is the first reader to access the shared resource, it locks access to the shared resource by waiting on the resource semaphore.
- After it has finished reading from the shared resource, it decrements the read_count variable, and if it is the last reader, it unlocks access to the shared resource by signaling the resource semaphore.
### Writer process:
- First, a writer acquires turn, if there are other readers accessing the shared resource the writer will wait till all of them exit, and since it has acquired turn, all other processes will have to wait for turn.
- It then waits on the resource semaphore to access the shared resource exclusively.
- After it gets access to shared resource, it gives up turn for others to acquire.
- After it has finished writing to the shared resource, it decrements the write_count variable, signals the resource semaphore to release the lock on the shared resource, and exits the queue.

  
// Reader process
void reader() {
    wait(turn);  // Acquire turn to be the next one to enter
    wait(read_mutex);  // Lock access to the read_count variable
    read_count++;  // Increment the number of readers accessing the shared resource
    if (read_count == 1) {  // If this is the first reader, lock access to the shared resource
        wait(resource);
    }
    signal(turn);  // Give turn so others can acquire
    signal(read_mutex);  // Unlock access to the read_count variable
    
    // Read from the shared resource
    
    wait(read_mutex);  // Lock access to the read_count variable
    read_count--;  // Decrement the number of readers accessing the shared resource
    if (read_count == 0) {  // If this is the last reader, unlock access to the shared resource
        signal(resource);
    }
    signal(read_mutex);  // Unlock access to the read_count variable
}

// Writer process
void writer() {
    wait(turn);  // Acquire turn to be next to enter
    wait(resource);  // Lock access to the shared resource
    signal(turn);  // Give turn so others can acquire
    
    // Write to the shared resource
   
    signal(resource);  // Unlock access to the shared resource
}
```
