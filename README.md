# Hash Hash Hash
Meagan Clarke, UID: 706058350

## Building
```shell
make clean
make
```

## Running
```shell
./hash-table-tester -t 4 -s 100000
Generation: 75,523 usec
Hash table base: 340,876 usec
  - 0 missing
```

## First Implementation
I initialized the mutex, which exists inside the hash_table_v1 struct, at line 36 when the hash table was created. I locked the mutex at the beginning of the `hash_table_v1_add_entry` function. I unlocked before returning (line 103), even in the case of existing list entry (line 92). This made the entire `hash_table_v1_add_entry` function a critical session, guaranteeing correctness because all operations are protected. 

### Performance
```shell
./hash-table-tester -t 4 -s 100000
Generation: 75,523 usec
Hash table base: 340,876 usec
  - 0 missing
Hash table v1: 705,610 usec
  - 0 missing
```
Version 1 is slower than the base version. As the there is increased overhead from acquiring and releasing the lock, combined with contention among threads waiting for the mutex. This leads to more time spent waiting and context switching, thereby reducing the efficiency of concurrent execution.

## Second Implementation
I initialized each mutex, which exist inside the hash_table_entry stuct, at line 35 when each hash_table_entry was being created. This means there exists as many mutexes as hash_table_entries. In the `hash_table_v1_add_entry` function, I locked the mutex of the particular hash_table_entry being accessed at line 83. I unlocked before returning (line 105), even in the case of existing list entry (line 94). This guarantees correctness because threads would not be able to access the same hash_table_entry at the same time. 

### Performance
```shell
./hash-table-tester -t 4 -s 100000
Generation: 75,523 usec
Hash table base: 340,876 usec
  - 0 missing
Hash table v1: 705,610 usec
  - 0 missing
Hash table v2: 100,407 usec
  - 0 missing
```

Version 2 is approximately 3.4x faster than the base version while being run with 4 cores. There is still increased overhead from acquiring and releasing the lock, but less contention among threads waiting for the mutex. This increases the efficiency of the concurrent execution.

## Cleaning up
```shell
make clean
```
