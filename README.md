# Ricart-Agrawala Algorithm Implementation (Ada)

## Project Overview
This repository contains a robust, strongly-typed implementation of the Ricart–Agrawala algorithm for achieving mutual exclusion in a distributed computing environment. It simulates the node-state logic, Lamport logical clocks, and tie-breaking mechanics required to safely enter a Critical Section (CS) without requiring a central coordinator.

## Features
* **Standard Variant**: Fully models the `2(N-1)` messaging requirement per CS entry.
* **Roucairol-Carvalho Optimization**: Implements implicit permission caching. Once a node receives a reply from another, it assumes permission is held until it must explicitly reply to a request from that same node, drastically reducing network overhead.
* **Safe Priority Tie-Breaking**: Resolves simultaneous requests deterministically using Lamport sequence numbers, falling back to unique Node IDs if sequence numbers match.
* **Strong Typing**: Uses strict custom Ada types (`Node_ID`, `Sequence_Number`, `Node_Set`) to prevent logic overlaps and arithmetic errors standard in integer-heavy C implementations.

## Testing
This project follows strict Verification & Validation (V&V) principles tailored for critical systems. The test suite operates on the pessimistic assumption that the code is non-functional, and tests only `PASS` when that assumption is provably disproved. 

### What the test categories verify:
* **Functional Correctness (Tests 1, 2, 4-6, 12-13):** Verifies that the internal state machine accurately mirrors the algorithm's standard rules (Released -> Wanted -> Held -> Released) and that barriers to entry correctly block execution.
* **Edge Cases & Tie Breaking (Tests 7, 8):** Verifies the resolution of simultaneous distributed requests, validating that the node correctly interprets sequence numbers and Node IDs as arbitration keys.
* **Algorithm Optimizations (Tests 9-11):** Verifies the stateful memory of the Roucairol-Carvalho variant, ensuring permissions are accurately cached and rigorously yielded when requested.

### Why these tests matter:
In distributed systems, deadlocks and starvation are catastrophic failure states. Validating that tie-breakers perfectly adhere to the deterministic logic ensures that mutual exclusion properties are maintained. By utilizing Ada's built-in `pragma Assert` capability during tests, we guarantee runtime safety bounds against these failure modes.

## Usage
Ensure you have the GNAT Ada compiler installed.

### Compilation
To compile both the main stub and the test suite:
```bash
make all
