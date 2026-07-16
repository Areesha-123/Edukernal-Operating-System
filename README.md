# EduKernel — Operating System Kernel Simulator

EduKernel is a desktop Java application that simulates the core responsibilities of an operating system kernel — process management, CPU scheduling, memory paging, and inter-process synchronization — through an interactive Swing GUI. It is designed as an educational tool for visualizing OS concepts taught in Operating Systems coursework.

## Overview

EduKernel provides a hands-on simulation environment where users can create processes, schedule them using different algorithms, allocate paged memory, and observe synchronization behavior between processes competing for a shared resource — all through a simple graphical interface rather than the command line.

## Features

### Process Management
- Create processes with configurable owner, priority, memory requirement, arrival time, and burst time
- **FCFS (First-Come-First-Served)** scheduling
- **Priority-based** scheduling
- Process lifecycle control: dispatch, block, resume, terminate, and destroy
- Inter-process messaging between processes by PID
- Live process table showing PID, state, owner, priority, memory, arrival/burst time, processor, I/O state, and messages
- Real-time CPU status and I/O device activity display

### Memory Management
- Paged memory allocation across a fixed pool of 16 frames
- Configurable page size (persisted to `memory_config.txt`)
- **LRU (Least Recently Used)** page replacement policy when memory is full
- Per-process page tables mapping logical pages to physical frames
- Memory deallocation on process termination
- Live frame-table view (Frame / PID / Page #) with manual refresh

### Synchronization
- Semaphore-based mutual exclusion for a shared printer resource
- Demonstrates the classic wait/signal (P/V) pattern with multiple processes competing for a single-instance resource

### Configuration
- In-app dialog to view and update the memory page size at runtime

## Architecture

The project follows a layered design that separates simulation logic from presentation:

```
edukernal/
├── src/
│   ├── Main.java                     # Application entry point
│   ├── core/
│   │   ├── Scheduler.java            # FCFS & priority scheduling, process queues
│   │   ├── MemoryManager.java        # Singleton; paging + LRU replacement
│   │   ├── Semaphore.java            # Classic counting semaphore (wait/signal)
│   │   └── Printer.java              # Shared resource guarded by a semaphore
│   ├── model/
│   │   ├── Process.java              # Process control block (PCB)
│   │   ├── Frame.java                # Physical memory frame
│   │   └── PageTableEntry.java       # Logical page → physical frame mapping
│   └── ui/
│       ├── ControlPanel.java         # Main window and navigation
│       ├── ProcessManagementUI.java  # Process creation, scheduling, controls
│       └── MemoryManagementUI.java   # Memory frame table view
├── memory_config.txt                 # Persisted page size configuration
└── edukernal.iml
```

### Design Patterns Used
- **Singleton** — `MemoryManager` maintains a single, shared view of physical memory across the application
- **Producer/consumer-style synchronization** — `Semaphore` + `Printer` model mutual exclusion over a shared resource

## Getting Started

### Prerequisites
- Java Development Kit (JDK) 8 or later
- Any Java IDE (IntelliJ IDEA project files are included) or a terminal with `javac`/`java`

### Running from an IDE
1. Open the project folder in IntelliJ IDEA (or your preferred IDE).
2. Run `src/Main.java`.

### Running from the command line
```bash
# From the project root
javac -d out $(find src -name "*.java")
java -cp out Main
```

## Usage

1. **Launch the app** — the main Control Panel opens with navigation buttons for Process Management, Memory Management, and Configuration.
2. **Process Management**
   - Click **Create Process** and fill in owner, priority, memory (MB), arrival time, and burst time. Memory is allocated automatically via the paging system.
   - Use **Run FCFS** or **Run Priority** to schedule and dispatch the next process to the CPU.
   - Use **Block** / **Resume** to simulate I/O waits, and **Terminate** / **Destroy** to end a running process (memory is deallocated automatically on termination).
   - Use **Send Message** to pass a message between two processes by PID.
   - Use **Use Shared Printer** to observe semaphore-based mutual exclusion when multiple processes request the printer concurrently.
3. **Memory Management** — opens a separate window showing the current frame table; click **Refresh** to update it after allocations or deallocations.
4. **Configuration** — set the page size (in MB) used for future memory allocations. Restart the app for changes to take effect.

## Configuration

The page size is stored in `memory_config.txt` at the project root as a single integer (default: `4`). This file is read on startup and can be updated through the **Configuration** dialog in the UI.

## Roadmap

Potential extensions for future development:
- Additional scheduling algorithms (Round Robin, SJF, Multilevel Queue)
- Deadlock detection/avoidance visualization
- Configurable frame count and multiple memory pools
- Unit tests for scheduler and memory manager logic
- Persistent process/session history

## Contributing

Contributions are welcome. Please open an issue to discuss proposed changes before submitting a pull request.

## License

Specify a license for this project (e.g., MIT) by adding a `LICENSE` file to the repository.
