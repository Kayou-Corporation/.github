# 📐 Kayou Corporation — C++23 Code Convention

> This document defines the coding standards for all Kayou Corporation projects (KEngine, KAlloc, KThreads).  
> Feel free to edit any section to match your team's preferences.

---

## Table of Contents

1. [General Philosophy](#1-general-philosophy)
2. [File Organization](#2-file-organization)
3. [Naming Conventions](#3-naming-conventions)
4. [Formatting & Style](#4-formatting--style)
5. [Classes & Structs](#5-classes--structs)
6. [Functions & Methods](#6-functions--methods)
7. [Variables & Constants](#7-variables--constants)
8. [Memory Management](#8-memory-management)
9. [Templates & Concepts](#9-templates--concepts)
10. [Error Handling](#10-error-handling)
11. [Threading](#11-threading)
12. [Comments & Documentation](#12-comments--documentation)
13. [Includes & Modules](#13-includes--modules)
14. [Vulkan-Specific Guidelines](#14-vulkan-specific-guidelines)

---

## 1. General Philosophy

- **Clarity over cleverness.** Code is read far more often than it is written.
- **Zero-overhead abstractions.** Prefer compile-time solutions over runtime indirection wherever possible.
- **Explicit is better than implicit.** Avoid surprising behavior; mark conversions and copies explicitly.
- **Modern C++23.** Use the latest standard features but only when they improve readability or performance, not for novelty.

---

## 2. File Organization

| Type | Extension |
|---|---|
| Header (declarations) | `.hpp` |
| Source (definitions) | `.cpp` |

- One class or closely related group of functions per header file.
- Mirror the directory structure with the namespace hierarchy.
- Public API headers go in `Include/`, private implementation headers in `Source/`.

```
KEngine/
├── Include/
│       ├── Core/
│       └── Renderer/
└── Source/
    ├── Core/
    └── Renderer/
```

---

## 3. Naming Conventions

### Namespaces

```cpp
namespace Kayou { }
namespace Kayou::Renderer { }
namespace Kayou::Memory { }
```

Put as little code as possible inside the `Kayou` namespace. Prefer nesting with a sub-namespace (e.g. `Kayou::Renderer` for rendering) <br/>
When using namespaces in sub-project, use the main-project name + sub-project name (e.g. `Kayou::Memory`) <br/>
Use `PascalCase` for namespaces. Avoid deeply nested namespaces (max 4 levels).

### Classes & Structs

```cpp
class RenderDevice { };
struct FrameData { };
```

Use `PascalCase`. Prefix engine core classes with nothing; use project prefixes (`KE`, `KA`, `KT`) only when building a public API.

### Interfaces / Abstract Classes

```cpp
class IRenderer { };        // Pure interface — prefix with I
class IAllocator { };
```

### Enumerations

```cpp
enum class ShaderStage : uint32_t
{
    Vertex   = 0,
    Fragment = 1,
    Compute  = 2,
};
```

Use `enum class` (scoped enums) only. Use `PascalCase` for the type and `PascalCase` for values.
Use explicit enumerator type when possible

### Functions & Methods

```cpp
void InitializeDevice();
uint32_t GetFrameIndex() const;
```

Use `PascalCase` for public methods. Use `PascalCase` for file-scope static helpers and prefix them with `Internal`.

### Variables

| Scope | Convention | Example |
|---|---|---|
| Local | `camelCase` | `frameCount` |
| Member | `m_camelCase` | `m_swapchain` |
| Static member | `s_camelCase` | `s_instance` |
| Global | `g_camelCase` | `g_device` |
| Constant / constexpr | `k_UPPER_SNAKE` | `k_MAX_FRAMES_IN_FLIGHT` |

### Macros

```cpp
#define KE_ASSERT(expr)         // Project prefix + UPPER_SNAKE_CASE
#define KA_ALIGN(x, alignment)
```

Minimize macro usage; prefer `constexpr`, `inline`, and templates.

---

## 4. Formatting & Style

- **Indentation:** 4 spaces (no tabs).
- **Line length:** Soft limit 100 characters, hard limit 120.
- **Brace style:** Allman (braces on their own line).

```cpp
if (condition)
{
    DoSomething();
}
else
{
    DoSomethingElse();
}
```

- **Spaces around operators:** `a + b`, `a == b`, `a && b`.
- **No trailing whitespace.**

---

## 5. Classes & Structs

- Declare `public` members first, then `protected`, then `private`. Do not repeat members (Only one `public`, `private` and `protected` member ber class)
- Use `struct` for plain data holders (POD-like types with no invariants).
- Use `class` for types that enforce invariants via encapsulation.
- Always declare a virtual destructor in base classes.
- Always declare overriden classes as `virtual` and `override` (e.g. `virtual RenderDevice() override`
- Mark single-argument constructors `explicit` unless implicit conversion is intentional.

```cpp
class RenderDevice
{
public:
    explicit RenderDevice(const DeviceCreateInfo& info);
    virtual ~RenderDevice();

    void Submit(CommandBuffer& cmd);
    [[nodiscard]] uint32_t GetVendorID() const;

protected:
    virtual void OnFrameBegin() { }

private:
    VkDevice         m_device     = VK_NULL_HANDLE;
    VkPhysicalDevice m_physDevice = VK_NULL_HANDLE;
};
```

---

## 6. Functions & Methods

- Keep functions short and focused (KISS & single responsibility).
- Prefer `KAYOU_NO_DISCARD` on functions that return error codes or resource handles.
- Use trailing return types when it improves readability.

```cpp
[[nodiscard]] auto CreateBuffer(const BufferCreateInfo& info) -> Buffer;
```

- Pass by `const&` for non-trivial read-only inputs; pass by value when the function needs a copy.
- Return by value; rely on NRVO/copy-elision.
- Avoid output parameters; prefer returning `std::expected` or a struct.

---

## 7. Variables & Constants

- Always initialize variables at declaration.
- Use `constexpr` over `const` for compile-time constants.
- avoid `auto` when possible to improve readability.

```cpp
constexpr uint32_t k_MAX_FRAMES_IN_FLIGHT = 3;

auto device = CreateDevice(info);          // WRONG — Even though the type is clear, explicit types makes code easily readable
uint32_t frameIndex = GetCurrentFrame();   // OK — always use explicit when possible
```

---

## 8. Memory Management

- **No raw `new`/`delete`** in engine code. Use KAlloc allocators or `std::pmr`.
- Prefer stack allocation; heap-allocate only when necessary.
- Smart pointer hierarchy:
  - `std::unique_ptr` — sole ownership (most common).
  - `std::shared_ptr` — shared ownership (use sparingly; prefer handle systems).
  - Raw pointers — non-owning observers only.
- Annotate non-owning raw pointers with `[[maybe_unused]]` observer comments where helpful.

```cpp
// Owning
std::unique_ptr<Buffer> m_vertexBuffer;

// Non-owning observer
RenderDevice* m_device = nullptr;  // not owned — lifetime managed by Engine
```

---

## 9. Templates & Concepts

- Use C++23 concepts to constrain templates; avoid unconstrained template parameters.

```cpp
template<typename T>
concept Allocator = requires(T a, std::size_t n)
{
    { a.Allocate(n)   } -> std::convertible_to<void*>;
    { a.Deallocate(nullptr, n) };
};

template<Allocator TAlloc>
class Pool { ... };
```

- Prefer `if constexpr` over template specialization for small branches.
- Keep template definitions in `.hpp` files.
- Avoid `.inl` files as they have no real benefits (in my opinion).

---

## 10. Error Handling

- **No exceptions** in hot paths (rendering loop, allocators, job system).
- Use `std::expected<T, Error>` (C++23) for fallible operations that return a value.
- Use assertion macros for programming errors (precondition violations).
- Use static assertion when it comes to compile-time checks.

```cpp
[[nodiscard]] std::expected<Buffer, VkResult> CreateBuffer(const BufferCreateInfo& info);

// Usage
auto result = CreateBuffer(info);
if (!result)
{
    KE_LOG_ERROR("Failed to create buffer: {}", result.error());
    return;
}
```

- Define a project-wide `Error` enum or error-code type per subsystem.

---

## 11. Threading

- Use KThreads primitives; avoid raw `std::thread` in engine code.
- Always document which thread(s) a function is safe to call from.
- Mark thread-safe functions with `// Thread-safe` in their doc comment.
- Prefer lock-free structures; document when locks are unavoidable.
- Never hold a lock across a Vulkan submission.

```cpp
/// Submits a render packet to the frame queue.
/// Thread-safe — may be called from any worker thread.
void SubmitRenderPacket(RenderPacket packet);
```

---

## 12. Comments & Documentation

- Use `///` Doxygen-style comments for all public API declarations and multiline comments.
- Use `//` for inline implementation notes.
- Do not comment the obvious; explain the *why*, not the *what*.

```cpp
/// @brief Allocates a contiguous block of \p size bytes aligned to \p alignment.
/// @param size       Number of bytes to allocate.
/// @param alignment  Required alignment (must be a power of two).
/// @return Pointer to the allocated block, or nullptr on failure.
[[nodiscard]] void* Allocate(std::size_t size, std::size_t alignment) noexcept;
```

---

## 13. Includes & Modules

- Include order (each group separated by a blank line):
  1. Corresponding `.hpp` for a `.cpp` file
  2. Project headers (alphabetical within each project)
  3. Third-party library headers
  4. Standard library headers
- Use `#pragma once` as the include guard.
- Minimize includes in headers; use forward declarations where possible.

```cpp
#pragma once

// Project headers
#include "KEngine/Core/Types.hpp"

// Third-party
#include <vulkan/vulkan.h>

// Standard library
#include <cstdint>
#include <span>
```

---

## 14. Vulkan-Specific Guidelines

- Name the central vulkan object of the class handle
```cpp
class VulkanDevice : 
vk::Device         m_handle     = VK_NULL_HANDLE;

class VulkanCommandPool : 
vk::CommandPool    m_handle    = VK_NULL_HANDLE;
```

- For any other vulkan object juste give it an explicit name
```cpp
class VulkanDevice : 
vk::Queue         m_queue     = VK_NULL_HANDLE;
```
  
- Always check Vulkan return codes; use a `VK_CHECK_RESULT` & `VK_CHECK_VOID` macro.

```cpp
#define VK_CHECK_RESULT(func, msg)                                                   \
    do {                                                                 \
        auto _res = (func);                                          \
        KE_ASSERT_MSG(_res.result == vk::Result::eSucess, "Vulkan error: {}", msg);    \
        return _res.value  \
    } while (false)

#define VK_CHECK_VOID(func, msg)                                                   \
    do {                                                                 \
        auto _res = (func);                                          \
        KE_ASSERT_MSG(_res.result == vk::Result::eSucess, "Vulkan error: {}", msg);   \
    } while (false)
```

- Destroy resources in the reverse order of creation.
- Use `vk::` (Vulkan-Hpp wrappers) or custom wrappers; never leave raw handles without a destructor.
- Synchronize CPU/GPU using timeline semaphores; avoid binary semaphores when possible.

---

*This document is a living reference. Submit a PR to propose changes.*
