---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.9.3"]
---

# DTrace (Short Notes)

1) DTrace is a dynamic tracing tool that can add probes to a running system (both user programs and the kernel) without restarting it.

2) It uses the **D language** to collect detailed information about system activity, kernel behavior, and process execution.

3) DTrace can trace interactions between user-space and kernel-space code, making complex debugging much easier.

4) Unlike traditional debugging methods (breakpoints, profiling, logging), DTrace works safely on live production systems with minimal performance impact.

5) DTrace consists of:

- **Providers** → create probes
- **Consumers** → use probe data
- **Compiler** → generates safe kernel bytecode
- **Framework** → manages probes and execution

6) When a probe is enabled, DTrace temporarily modifies the target code to collect data; when disabled, the original code is restored.

7) DTrace uses **ECBs (Enabling Control Blocks)**, which act like "if statements + actions" that run when a probe fires. They can collect variables, timings, and other runtime information.

8) DTrace can connect user-level actions with the kernel activities they trigger, making it valuable for debugging, performance analysis, and optimization.

9) Safety mechanisms limit CPU and memory usage. If a DTrace program consumes too many resources, it is automatically terminated.

10) DTrace originated in Solaris and was later adopted by systems such as macOS and FreeBSD. Modern Linux systems typically use **eBPF** for similar tracing and observability tasks.

## Related

- [[> Operating-System Debugging]] — back to the sub-topic MOC
- [[Performance Tuning]] — where DTrace is introduced
- [[Failure Analysis]] — the other debugging task
