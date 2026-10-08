# Bite Chu

**Target Position: Game Developer / Game Client Engineer (C++)**

Email: your.email@example.com | Phone: +86-XXX-XXXX-XXXX | GitHub: github.com/your-id

## Education

### KTH Royal Institute of Technology (QS Top 100)

| Degree | Location | Period |
| --- | --- | --- |
| M.Sc. in Software Engineering | Stockholm, Sweden | Aug 2024 – Dec 2026 (Expected) |

- **Core Courses:** Distributed Systems, Software Engineering, Data Mining, Machine Learning
- **Honors:** KTH One-Year Scholarship (2025, top 5%)

### Xi'an Jiaotong University (C9 League)

| Degree | Location | Period |
| --- | --- | --- |
| B.Eng. in Computer Science and Technology | Xi'an, China | Sep 2020 – Jun 2024 |

- **Core Courses:** Data Structures & Algorithms, Computer Organization, Computer Networks, Operating Systems, Databases, Artificial Intelligence
- **Honors:** Uniqlo Scholarship (2022, top 30%); XJTU University Scholarship (2021, top 10%)

## Internship Experience

### ChipON Microelectronics — Compiler Development Intern

| Role | Location | Period |
| --- | --- | --- |
| Compiler Development Intern | Shanghai, China | Mar 2026 – Jun 2026 |

Worked on the LLVM backend assembler for the company's in-house KF32R RISC instruction set architecture, covering automated testing, instruction-parsing debugging, and new feature support.

- Built an automated assembly/disassembly verification pipeline for the KF32R ISA (130+ instructions) with the LLVM toolchain (`llvm-mc` / `llvm-objdump`), using FileCheck to verify round-trip consistency of "assembly → machine code → disassembly".
- Systematically designed valid and invalid instruction test cases covering register out-of-range, immediate overflow, and malformed syntax, improving instruction verification coverage.
- Contributed to LLVM backend development by modifying TableGen descriptions and the AsmParser, adding pseudo-instruction expansion and PC-relative branch support.
- Gained hands-on understanding of register models, calling conventions, branch relaxation, and linker relocation.

### Fast Retailing (UNIQLO) China Trading Co., Ltd. — Commercial IT Specialist Intern

| Role | Location | Period |
| --- | --- | --- |
| Commercial IT Specialist Intern | Shanghai, China | Jan 2024 – Mar 2024 |

Joined the Commercial IT team building an O2O (Online-to-Offline) system that connects UNIQLO's offline stores with the TikTok (Douyin) e-commerce platform to drive online sales.

- Performed end-to-end functional testing of core O2O workflows, including online ordering, logistics tracking, returns/exchanges, and inventory management, reporting and tracking defects with developers until resolution.

## Projects

### AvaRat — 2D Side-Scrolling Runner Game (Unity)

**Period:** Mar 2025 – Jun 2025 | **Type:** Team project (5 members) | **Tech Stack:** Unity, C#, React, Firebase, GitHub

**Link:** [https://avarat-add8a.web.app](https://avarat-add8a.web.app)

A runner game in which the player controls a rat that switches between *Ice* and *Fire* forms to overcome terrain and obstacles: Ice form freezes liquids to gain speed, while Fire form ignites gas to trigger explosions for an extra jump.

- Implemented the Ice/Fire form-switching mechanic and the character–environment interaction logic in Unity.
- Built and deployed the game's showcase website with React + Firebase.
- Participated in the full product cycle, from gameplay ideation, user analysis, and market research to web deployment and pricing.

### Distributed Key-Value Store Based on Raft

**Period:** Jun 2026 – Jul 2026 | **Type:** Personal project | **Tech Stack:** C++, Protobuf, Multithreading

A distributed KV database built on the Raft consensus algorithm, providing linearizability and partition tolerance; the service remains available as long as a majority of nodes are alive. Uses a self-implemented RPC framework (MprRpc) and skip-list storage engine (SkipListPro).

- Implemented Raft heartbeats and leader election, triggered by a timer-driven thread pool, and maintained the cluster's log commit state.
- Implemented log replication: the leader handles client requests, replicates log entries to followers, commits once a majority acknowledges, applies commands to the state machine, and responds to the client.
- Built MprRpc, an RPC framework based on Protobuf and a custom wire protocol, for remote calls and data transfer between nodes.
- Implemented SkipListPro, a skip-list-based KV storage engine.
- Designed the client protocol with a unique request ID (client IP + sequence number) to guarantee linearizability, together with client-side retry.

## Technical Skills

- **C/C++:** Solid understanding of OOP (encapsulation, inheritance, polymorphism), STL containers, and common C++11 features.
- **Data Structures & Algorithms:** Stacks, queues, linked lists, hash tables, binary trees; binary search, backtracking, greedy, dynamic programming, quicksort, etc.
- **Computer Networks:** TCP/IP model; HTTP, TCP/UDP, including TCP three-way handshake and four-way termination.
- **Operating Systems:** Memory management, process scheduling, inter-process communication, synchronization and mutual exclusion.
- **Tools:** Unity, LLVM (TableGen, llvm-mc, FileCheck), Git, Protobuf; proficient with AI coding agents such as Claude Code and Codex.

## Gaming Experience

- **Honor of Kings** (5,000+ hrs): Nearly ten years of play since launch; peak rank Glorious King 100 stars, Peak Tournament rating 1800.
- **CrossFire** (500+ hrs): Long-term play through middle and high school; extensive hands-on FPS experience.
- **Red Dead Redemption 2** (200+ hrs): Completed the first playthrough; deeply impressed by its immersive world, narrative, and characterization — one of my all-time favorites.
- **Dead Cells** (150+ hrs): Currently at 1 Boss Cell; most drawn to its fluid combat system and satisfying hit feedback.
- **Octopath Traveler II** (80+ hrs): A classic JRPG experience; especially love its music and storytelling.
- **Forza Horizon 4** (50+ hrs): Enjoy free driving, open-world exploration, and the scenery.
- **Ori and the Will of the Wisps** (50+ hrs): A favorite 2D action-adventure with outstanding music, narrative, and art.
- **Dave the Diver** (50+ hrs): A novel blend of genres; the pacing shifts between systems keep it consistently fresh.
- **It Takes Two** (30+ hrs): Completed in co-op with a friend; the cooperative mechanics and level interactions are a blast.

## Summary

Strong CS fundamentals and hands-on engineering experience, with a B.Eng. in Computer Science from Xi'an Jiaotong University and an ongoing M.Sc. at KTH focusing on distributed systems. Experienced in LLVM toolchain development and distributed KV storage, with in-depth understanding of C/C++, operating systems, computer networks, and distributed systems. A fast learner and effective debugger who picks up unfamiliar technologies quickly and enjoys digging into low-level principles. Responsible, proactive in communication, and open to feedback — eager to grow as an engineer and deliver long-term value to the team.
