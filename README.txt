# Phantom Pursuit Simulator

A multithreaded ghost-hunting simulation built in C, demonstrating concurrent programming with POSIX threads, mutex synchronization, and shared memory management.

Four hunters explore a procedurally structured house, collecting evidence while a ghost roams between rooms. Each entity runs on its own thread, coordinating access to shared data structures through mutexes.

## Key Concepts Demonstrated
- POSIX thread creation and lifecycle management
- Mutex locks for shared resource access (rooms, evidence lists)
- Producer/consumer pattern between ghost and hunter threads
- Dynamic memory allocation and cleanup across threads

## Build & Run
```bash
make
./a5
```
Enter 4 hunter names when prompted.

## Files
| File | Purpose |
|------|---------|
| main.c | Entry point, initialization |
| ghost.c | Ghost thread logic |
| hunter.c | Hunter thread logic |
| room.c | Room/house structure |
| evidence.c | Evidence collection |
| logger.c | Thread-safe logging |
