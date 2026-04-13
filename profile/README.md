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

KEngine is our flagship project — a fully custom, data-driven 3D game engine written in modern C++23. It is designed with performance, modularity, and multi-API rendering in mind.

- **Rendering back-end:** Vulkan (DirectX 12 planned)
- **Architecture:** Entity-Component-System (ECS)
- **Scene management, asset pipeline, physics integration and more**

---

### 🧠 KAlloc
> Custom memory allocator library.

KAlloc provides a suite of high-performance, low-level memory allocators tailored for real-time game engine workloads:

- Pool allocator, stack allocator, free-list allocator
- NUMA-aware and cache-line-aligned allocations
- Fully compatible with C++23 allocator concepts and `std::pmr`

---

### 🧵 KThreads
> Custom threading and task system.

KThreads is our multi-threading layer built for deterministic, high-throughput workloads:

- Lock-free job/task queue
- Fiber-based coroutine support
- Thread-affinity and priority management
- Seamless integration with KEngine's frame scheduler

---

## 📐 Code Convention

All Kayou Corporation projects follow a unified C++23 code convention.  
→ See [`CODE_CONVENTION.md`](../CODE_CONVENTION.md) for the full reference.

---

## 🤝 Contributing

We welcome contributions from the community!  
Please read [`CONTRIBUTING.md`](../CONTRIBUTING.md) before opening a pull request.

---

<div align="center">
  <sub>Built with ❤️ by the Kayou Corporation team.</sub>
</div>
