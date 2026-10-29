# Phantom Pursuit Simulator

A multithreaded ghost-hunting simulation in C. Four hunters and one ghost each run on their own POSIX thread inside a shared 13-room house. The hunters try to collect enough evidence to identify the ghost before fear or boredom drives them out.

## How it works

- **The ghost** starts in a random room as one of four types: Poltergeist, Banshee, Bullies or Phantom. Each type leaves three of the four kinds of evidence (EMF, temperature, fingerprints, sound). The ghost wanders between connected rooms and drops evidence as it goes.
- **The hunters** start in the Van. Each one carries a single piece of equipment and can only pick up the matching kind of evidence. On every turn a hunter moves, collects evidence or checks the shared evidence list.
- **Fear and boredom.** A hunter's fear goes up while they share a room with the ghost, and their boredom goes up while they don't. A hunter leaves when either one reaches its limit. The ghost also leaves if it goes too long without meeting a hunter.
- **Ending.** Once the hunters have three distinct pieces of evidence, the program names the ghost type. If every hunter flees first, the ghost wins.

## Concurrency

- One thread per entity: 4 hunter threads and 1 ghost thread, created with `pthread_create` and joined in `main`.
- Each room has its own binary semaphore (`sem_t`) protecting its hunter list, ghost pointer and evidence list. A move between rooms locks both rooms.
- Evidence flows one way: the ghost leaves it in rooms and the hunters take it from there into a single shared list.
- All memory (rooms, lists, hunters, ghost) is allocated dynamically and freed once every thread has finished.

## Build and run

Requires `gcc`, `make` and a POSIX system (Linux or macOS).

```bash
make
```

```bash
./a5
```

Enter four hunter names when prompted. Every action is logged to stdout as it happens. Run `make clean` to remove build artifacts.

Tuning constants (thread sleep times, fear and boredom limits, hunter count) are in `defs.h`.

## Project layout

| File | Purpose |
|------|---------|
| `main.c` | Entry point: setup, thread creation and joining, final result |
| `defs.h` | Constants, types and function prototypes |
| `house.c` | House setup: room creation, connections and cleanup |
| `room.c` | Room and room-list management |
| `ghost.c` | Ghost thread, evidence generation, ghost-type identification |
| `hunter.c` | Hunter thread, equipment and movement |
| `evidence.c` | Evidence linked list |
| `logger.c` | Formatted event logging |
| `utils.c` | Random number helpers and enum-to-string conversion |
