# 🤝 Contributing to Kayou Corporation Projects

Thank you for your interest in contributing to our projects!  
Please take a few minutes to read through these guidelines before opening an issue or pull request.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Reporting Bugs](#reporting-bugs)
- [Requesting Features](#requesting-features)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Code Convention](#code-convention)
- [Commit Messages](#commit-messages)

---

## Code of Conduct

By participating in any Kayou Corporation project you agree to treat all contributors with respect. Harassment, discrimination, or disruptive behavior of any kind will not be tolerated.

---

## Getting Started

1. **Fork** the repository and clone your fork locally.
2. Create a new branch off `main` for your change:
   ```bash
   git checkout -b feat/my-feature
   ```
3. Make your changes, following the [Code Convention](CODE_CONVENTION.md).
4. Build the project and ensure all tests pass before pushing.
5. Open a pull request against the `main` branch.

---

## Reporting Bugs

Use the **Bug Report** issue template. Please include:

- A clear description of the problem.
- Steps to reproduce the issue.
- Expected vs. actual behavior.
- Version / commit SHA, OS, GPU, and driver version.

---

## Requesting Features

Use the **Feature Request** issue template. Please describe:

- The problem you are trying to solve.
- Your proposed solution (if any).
- Any alternative approaches you considered.

---

## Submitting a Pull Request

- Keep PRs focused — one feature or fix per PR.
- Reference the related issue in the PR description (e.g., `Closes #42`).
- Add or update tests when fixing a bug or adding a feature.
- Ensure the CI pipeline passes before requesting a review.
- All PRs require at least **one approving review** before merging.

---

## Code Convention

All contributions must follow our [C++23 Code Convention](CODE_CONVENTION.md).  
Run `clang-format` on your changes before pushing.

---

## Commit Messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <short description>

[optional body]

[optional footer(s)]
```

**Types:** `feat`, `fix`, `perf`, `refactor`, `docs`, `test`, `chore`, `ci`

**Examples:**

```
feat(renderer): add Vulkan timeline semaphore support
fix(kalloc): prevent double-free in pool allocator reset
docs: update code convention threading section
```
