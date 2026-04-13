<div align="center">

# 🎮 Kayou Corporation

**Building a high-performance, custom 3D game engine from the ground up — powered by C++23 and Vulkan.**

[![Language](https://img.shields.io/badge/language-C%2B%2B23-blue?style=flat-square)](https://en.cppreference.com/w/cpp/23)
[![Graphics API](https://img.shields.io/badge/graphics-Vulkan-red?style=flat-square)](https://www.vulkan.org/)
[![License](https://img.shields.io/badge/license-Proprietary-lightgrey?style=flat-square)](#)

</div>

---

## 🚀 Projects

### 🔧 KEngine
> The core 3D game engine.

KEngine is our main project — a fully custom, 3D game engine written in modern C++23. It is designed with performance, modularity, and multi-API rendering in mind.

- **Rendering back-end:** Vulkan (DirectX 12 planned)
- **Architecture:** Entity-Component-System (ECS)
- **Scene management, asset pipeline, physics integration and more**

---

### 🧵 KThreads
> Custom threading and task system.

KThreads is our multi-threading layer built for deterministic, high-throughput workloads:

- Lock-free job/task queue
- Fiber-based coroutine support
- Thread-affinity and priority management
- Seamless integration with KEngine's frame scheduler

---

### 🧠 KAlloc
> Custom memory allocator library.

KAlloc provides a suite of high-performance, low-level memory allocators tailored for real-time game engine workloads:

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
  <sub>Built with ❤️ for the Kayou.</sub>
</div>
