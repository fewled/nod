## Onesheet
[Back](https://github.com/fewled/nod)

<br>

### High-Level Objective

To implement the Raft Distributed Consensus Protocol in Rust, focusing on the core components: Leader Election, Log Replication, and Safety. The project will emphasize "low-level" networking and minimal dependencies to showcase systems programming proficiency.

<br>

### Architecture & Environment

The deployment strategy is a "Cluster-in-a-Box" model:
- **Orchiestrator (`cluster`)**: A Rust binary acting as the parent process/container. It manages the lifecycle of the nodes and simulates the network topology.
- **Nodes (`node` & `raft`)**: Individual instances running the Raft logic. Each will be a FROM scratch Docker container to keep the footprint minimal.
- **The "Seed-Leader" Pattern**: Upon initialization, the first node (Seed) handles the registration of subsequent nodes until a quorum is reached to begin formal elections.

<br>

### Networking Strategy

To maximize learning and optimization, we will bypass high-level abstractions:
- **Transport**: Raw TCP/UDP sockets (using `std::net` or `tokio::net`). No gRPC or HTTP frameworks.
- **Serialization**: Manual byte-level serialization or a lightweight binary format (like Bincode) rather than JSON, to keep the "wire" footprint small.
- **Fault Injection**: The cluster orchestrator will have the power to "cut" connections between specific node IPs to simulate network partitions (split-brain scenarios).

<br>

### Technical Boundaries & Scope

- **Log Persistence**: Implementation of an append-only log that survives node restarts.
- **State Machine**: A simple Key-Value store to demonstrate that the replicated log actually does something.
- **Heartbeats & Timeouts**: Randomized election timeouts to prevent perpetual split votes.
- **Log Compaction (Snapshoting)**: Usually the most tedious part of Raft; save this for "Phase 2."
- **Dynamic Membership**: We will start with a fixed number of nodes (e.g., 3 or 5) defined at startup rather than adding nodes on the fly.
