# Phase 0: Lab Environment Documentation

## Purpose

Phase 0 establishes the foundational lab environment and documents the baseline infrastructure. This phase does not introduce Tetragon, Kubernetes, or eBPF development tooling yet; instead, it captures the state of the underlying Ubuntu system and verifies the prerequisites that matter for later runtime-security work.

The lab is a dedicated physical laptop, separate from the development workstation used for GitHub, VS Code, Codex, and documentation. All runtime experiments and observations are performed on the lab machine.

## Current Environment

**Lab Machine Type:** Dedicated physical laptop

**Operating System:** Ubuntu Server 24.04 LTS with a GUI installed afterwards

**Purpose:** Dedicated experimental environment for Linux runtime-security research and, in later phases, Tetragon detection engineering

### Hardware & Kernel Configuration

| Property | Value |
|----------|-------|
| Ubuntu Release | `[PLACEHOLDER: confirm with cat /etc/os-release or lsb_release -d]` |
| Kernel Version | `[PLACEHOLDER: confirm with uname -r]` |
| Architecture | `[PLACEHOLDER: confirm with uname -m]` |
| CPU Count | `[PLACEHOLDER: confirm with nproc or lscpu]` |
| Memory (Total) | `[PLACEHOLDER: confirm with free -h]` |
| Root Filesystem Capacity / Usage | `[PLACEHOLDER: confirm with df -h /]` |

*These values must describe the dedicated lab machine, not the development workstation or any WSL environment.*

## Software / Tools

Only tools actually verified on the lab machine should be listed here as installed. Ubuntu package contents vary by installation choices, so this document does not assume that compilers, Git, Python, editors, or tracing tools are present merely because the system is Ubuntu Server.

### Verified Base Components

| Component | Status |
|-----------|--------|
| Ubuntu Server 24.04 LTS | Installed |
| Graphical desktop environment | Installed after the server installation |
| Package management (`apt` / `dpkg`) | Available |
| Additional development / tracing tools | Document as they are verified or installed |

### Tools Introduced in Later Phases

These are planned tools, not Phase 0 prerequisites:

| Tool | Purpose | Introduced when needed |
|------|---------|------------------------|
| `strace` | System-call tracing | Linux processes / syscalls work |
| `bpftrace` | Interactive eBPF tracing | eBPF fundamentals |
| `bpftool` | Inspect BPF objects and kernel BPF capabilities | eBPF fundamentals |
| container runtime | Container experiments | Containers phase |
| K3s | Lightweight single-node Kubernetes | Kubernetes phase |
| Clang / LLVM and related eBPF tooling | eBPF program development | eBPF phase |
| Cilium Tetragon | Runtime observability and enforcement | Tetragon phase |
| Go | Tetragon source exploration and tooling | Later source/contribution work |

Tools will be introduced progressively so that each dependency has a clear purpose and its configuration can be documented deliberately.

## Kernel and eBPF Readiness

Kernel version alone is not enough to prove that a system is suitable for all eBPF or Tetragon functionality. Relevant kernel configuration, available attachment mechanisms, BTF data, privileges, and the features used by a specific experiment all matter.

Phase 0 therefore records a small number of non-invasive readiness signals rather than treating eBPF support as a single yes/no property.

### Readiness Checks

| Check | Why it matters | Status |
|-------|----------------|--------|
| Kernel version | Establishes the kernel baseline for later feature checks | `[PLACEHOLDER]` |
| BTF file at `/sys/kernel/btf/vmlinux` | Strong indicator that kernel BTF information is exposed to BPF tooling | `[PLACEHOLDER]` |
| `/sys/fs/bpf` availability | Identifies the BPF filesystem location used by later tooling | `[PLACEHOLDER]` |
| Unprivileged BPF policy | Helps explain which operations require elevated privileges | `[PLACEHOLDER]` |

### Verification Command Reference

Run these commands **on the dedicated lab machine** and record the actual results rather than inferring them from the distribution name:

```bash
# Distribution and kernel baseline
cat /etc/os-release
uname -r
uname -m

# Hardware baseline
nproc
free -h
df -h /

# BTF exposed by the running kernel
if [ -e /sys/kernel/btf/vmlinux ]; then
  echo "BTF available"
else
  echo "BTF not found"
fi

# BPF filesystem (may be mounted later by tooling if not already present)
mount | grep ' /sys/fs/bpf ' || true

# Kernel policy for unprivileged BPF, when exposed
cat /proc/sys/kernel/unprivileged_bpf_disabled 2>/dev/null || echo "setting not exposed"
```

`bpftool` is intentionally not required for the Phase 0 check because it may not yet be installed. It will become useful later when we intentionally introduce BPF tooling.

## Design Decisions

### Dedicated Single-Machine Lab

**Decision:** Use one dedicated physical Linux machine instead of a multi-node Kubernetes or multi-VM environment.

**Rationale:**
1. **Focus:** Linux runtime behaviour can be studied without unrelated cluster complexity.
2. **Reproducibility:** Fewer moving parts make experiments easier to repeat and explain.
3. **Resource efficiency:** The project can progress on modest hardware.
4. **Isolation:** Potentially disruptive experiments remain separate from the primary workstation.
5. **Progressive complexity:** Containers, Kubernetes, eBPF, and Tetragon are added only after the underlying Linux concepts are understood.

### Separation of Workstation and Lab

**Decision:** Keep development/documentation work and runtime experimentation on separate machines.

**Rationale:**
- The development workstation contains the GitHub clone, VS Code/Codex workflow, and polished documentation.
- The Ubuntu laptop is the system whose processes, kernel behaviour, containers, and later Kubernetes workloads are observed.
- Results copied into the repository should identify which machine produced them.
- The separation reduces the risk of accidentally documenting workstation or WSL state as lab evidence.

## Verification

Phase 0 is complete when the following are true:

- [ ] The dedicated lab laptop is confirmed to be running Ubuntu Server 24.04 LTS with the added GUI.
- [ ] Kernel version, architecture, CPU, memory, and filesystem baseline are recorded above.
- [ ] BTF availability has been checked on the lab machine.
- [ ] Basic networking is available; SSH may be enabled if remote administration is desired.
- [ ] The lab is not being used for production or critical workloads.
- [ ] All values in this document reflect the lab machine rather than the development workstation.

## Future Evolution

The environment will evolve only when a learning objective requires it. Planned additions include:

1. Linux tracing and process-inspection tools.
2. Container-runtime experiments.
3. Single-node K3s.
4. eBPF tracing and development tooling.
5. Tetragon and `tetra`.
6. Reproducible detection workloads and tests.

When the environment changes, record tool versions or configuration details **when they materially affect reproducibility or observed behaviour**. Avoid turning this file into a complete package inventory.
