<div align="center">

# 🎮 Kayou Corporation

**Building a high-performance, custom 3D game engine from the ground up — powered by C++23 and Vulkan.**

[![Languages](https://img.shields.io/badge/Language-C%2B%2B23-lime?style=for-the-badge&logo=cplusplus)](https://en.cppreference.com/w/cpp/23)
[![test](https://img.shields.io/badge/language-SLANG-orange?style=for-the-badge)](https://shader-slang.org/)
[![Graphics API](https://img.shields.io/badge/Graphics-Vulkan-darkred?style=for-the-badge&logo=vulkan)](https://www.vulkan.org/)
[![License](https://img.shields.io/badge/License-Open_Source-blue?style=for-the-badge)](#)

</div>

---

## 🚀 Projects

### 🔧 [KEngine](https://github.com/Kayou-Corporation/KEngine)
> Our main project — a fully custom, 3D game engine written in modern C++23. It is designed with performance, modularity, and multi-API rendering in mind.

- **Rendering back-end:** Vulkan (DirectX 12 planned)
- **Architecture:** Entity-Component-System (ECS)
- **Scene management, asset pipeline, physics integration and more**

---

### 🧵 [KThreads](https://github.com/Kayou-Corporation/KThreads)
> KThreads is our multi-threading layer built for deterministic, high-throughput workloads

- Single thread management with polls of tasks
- Thread pool management that can hold multiple queues, each with its own reserved threads
- Priority management for each queue of a pool, priorities being "High" or "Low"

---

### 🧠 [KAlloc](https://github.com/Kayou-Corporation/KAlloc)
> KAlloc provides a suite of high-performance, low-level memory allocators tailored for real-time game engine workloads

- Based on C++23
- Aligned allocations & allocator-specific strategies (linear, pool, stack)
- Tracking and debug validation
- Optional [Tracy](https://github.com/wolfpld/tracy) profiling integration

---

## 📐 Code Convention

All Kayou Corporation projects follow our unified C++23 code convention.  
→ See [`CODE_CONVENTION.md`](../CODE_CONVENTION.md) for the full reference.

---

## 🤝 Contributing

We welcome any contribution and would be more than happy to discuss our project with anyone willing to!  
If you wish to contribute, please read [`CONTRIBUTING.md`](../CONTRIBUTING.md) before opening a pull request.

---

<div align="center">
  <sub>As these projects are mainly for learning purposes, we try to avoid the usage of (generative) AI as much as possible.</sub>
</div>
<div align="center">
  <sub>Built with ❤️ for the Kayou.</sub>
</div>

