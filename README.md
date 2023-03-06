# Readers_Writers_Problem
The Readers-Writers problem is a classical synchronization problem, where multiple threads are competing for access to a shared resource, with different types of access modes (Reader/Writer). In this problem there are two types of threads: readers and writers. Readers only read from the shared resource, therefore any number of readers can access at the same time. Whereas writers modify the shared resource, therefore only one writer can access at a time (no readers are allowed to read during writing). The goal is to ensure that readers and writers can access the shared resource safely and efficiently.
## Solution
The solution to the Readers-Writers problem presented here is a starve-free solution, which means that it guarantees that no thread will have to wait indefinitely. This solution is implemented using mutexes and condition variables for synchronization
5 variables have been used in the pseudocode: read_count, write_count, read_mutex, write_mutex, turn.
1. 'read_count' keeps a track of the number of active reader processes, it is initialised to zero.
```
int read_count = 0;
```
2. 'write_count' keeps a track of the number of active writer processes, it is initialised to zero.
```
int write_count = 0;
```
3. 'read_mutex' is a binary semaphore which protects the read_count variable.
4. 'write_mutex' is a binary semaphore which protects the write_count variable.
5. 'turn' is a semaphore used at the beginning of both the reader and writer codes. Any reader/writer has to acquire this semaphore in order to enter the critical section, each having equal priority of doing so.
```
semaphore read_mutex=1, write_mutex=1, turn=1;
```
## Pseudocode
```
// Global variables
int read_count = 0;
int write_count = 0;
semaphore read_mutex=1, write_mutex=1, turn=1;

  
// Reader process
void reader() {
    wait(queue);  // Enter the queue to access the shared resource
    wait(read_mutex);  // Lock access to the read_count variable
    read_count++;  // Increment the number of readers accessing the shared resource
    if (read_count == 1) {  // If this is the first reader, lock access to the shared resource
        wait(resource);
    }
    signal(queue);  // Exit the queue
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
    wait(queue);  // Enter the queue to access the shared resource
    wait(resource);  // Lock access to the shared resource
    signal(queue);  // Exit the queue
    write_count++;  // Increment the number of writers accessing the shared resource
    // Write to the shared resource
    write_count--;  // Decrement the number of writers accessing the shared resource
    signal(resource);  // Unlock access to the shared resource
}
```
