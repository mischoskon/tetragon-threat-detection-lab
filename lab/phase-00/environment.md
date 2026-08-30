# Phase 0: Lab Environment Documentation

## Purpose

Phase 0 establishes the foundational lab environment and documents the baseline infrastructure. This phase does not introduce Tetragon, Cilium, Kubernetes, or eBPF tools yet; instead, it captures the state of the underlying Ubuntu system and verifies prerequisites for future phases.

The lab environment is a dedicated physical machine where all experiments and observations will occur. This document records configuration, capabilities, and design decisions for reproducibility.

## Current Environment

**Lab Machine Type:** Physical or dedicated virtual machine (separate from development workstation)

**Operating System:** Ubuntu Server 24.04 LTS with GUI installed afterwards

**Purpose:** Dedicated experimental environment for runtime security research and Tetragon detection engineering

### Hardware & Kernel Configuration

| Property | Value |
|----------|-------|
| Ubuntu Release | `[PLACEHOLDER: To be confirmed from `lsb_release -d` on lab machine]` |
| Kernel Version | `[PLACEHOLDER: To be confirmed from `uname -r` on lab machine]` |
| Architecture | `[PLACEHOLDER: To be confirmed from `uname -m` on lab machine]` |
| CPU Count | `[PLACEHOLDER: To be confirmed from `nproc` on lab machine]` |
| Memory (Total) | `[PLACEHOLDER: To be confirmed from `free -h` on lab machine]` |
| Disk Space (Available) | `[PLACEHOLDER: To be confirmed from `df -h` on lab machine]` |

*Note: These values describe the **lab machine only**, not the development workstation.*

## Software / Tools

### Pre-installed (Ubuntu 24.04 LTS baseline)
- **Package Manager:** apt/dpkg
- **Shell:** bash (typically)
- **Build Tools:** gcc, make, git (standard Ubuntu server)
- **Python:** Python 3.x (standard Ubuntu 24.04)
- **Text Editors:** nano, vim

### Required for Upcoming Phases (Not yet installed)

| Tool | Purpose | Phase | Status |
|------|---------|-------|--------|
| **strace** | System call tracing | Phase 1 | Not installed |
| **perf** | Performance and kernel profiling | Phase 1 | Not installed |
| **Docker** or **containerd** | Container runtime | Phase 2 | Not installed |
| **K3s** | Lightweight Kubernetes | Phase 3 | Not installed |
| **eBPF tools** (clang, llvm, libbpf) | eBPF program development | Phase 4 | Not installed |
| **Cilium Tetragon** | Runtime security platform | Phase 4 | Not installed |
| **Go** (optional) | Tetragon and tool development | Phase 4 | Not installed |

### Notes
- Do not install additional software as part of Phase 0
- Tools will be installed progressively as they are needed in subsequent phases
- All installations should be documented with version numbers and configuration steps

## Kernel and eBPF Readiness

### Kernel Requirements

The lab machine's kernel must support:
- **eBPF programs** (Linux 5.0+)
- **BPF LSM** (Linux 5.8+, if used)
- **Tracepoints and kprobes** (most modern kernels)
- **BTF (BPF Type Format)** support (recommended for Tetragon, Linux 5.8+)

### Readiness Check

| Capability | Required | Status |
|------------|----------|--------|
| eBPF Support | Yes | `[PLACEHOLDER: To be verified with `bpftool version` on lab machine]` |
| BTF Support | Recommended | `[PLACEHOLDER: To be verified with `bpftool btf list` on lab machine]` |
| eBPF JIT | Recommended | `[PLACEHOLDER: To be verified from `/proc/sys/kernel/bpf_jit_enable` on lab machine]` |
| Unprivileged eBPF | No | `[PLACEHOLDER: Lab assumes root or CAP_SYS_ADMIN availability]` |

### Verification Command Reference

Once the lab machine is accessible, verify eBPF readiness with:

```bash
# Check kernel version
uname -r

# Check BTF support
bpftool btf list

# Check eBPF availability
cat /proc/sys/kernel/unprivileged_bpf_disabled

# Check JIT compilation
cat /proc/sys/kernel/bpf_jit_enable
```

## Design Decisions

### Single Lab Machine (Not Multi-Node)

**Decision:** Use a single dedicated machine rather than a cluster or multi-VM environment.

**Rationale:**
1. **Learning Progression**: Deep understanding of single-node Linux fundamentals is prerequisite to container/cluster concepts
2. **Reproducibility**: Simpler state management and easier to document exact conditions
3. **Cost Efficiency**: Minimal infrastructure overhead
4. **Clarity**: Fewer moving parts allow focus on observability patterns
5. **Scalability Path**: Future phases can introduce containers and K3s once Linux foundations are solid

### Separation of Concerns

**Decision:** Maintain clear distinction between development workstation and lab environment.

**Rationale:**
- Repository and documentation live on the development machine
- Experiments and raw observations occur on the dedicated lab machine
- Reduces accidental contamination of lab environment
- Simulates real security operations: a SOC analyst maintains logs/documentation separately from monitored infrastructure

### No Initial Software Beyond Ubuntu Base

**Decision:** Phase 0 installs no additional tools beyond standard Ubuntu 24.04 LTS.

**Rationale:**
- Establishes clean baseline
- Each tool introduction in future phases is documented and justified
- Allows verification of lab readiness before complex tooling

## Verification

Use the following checklist to verify Phase 0 completion:

- [ ] Lab machine is running Ubuntu Server 24.04 LTS with GUI
- [ ] Lab machine has network connectivity and SSH access (if remote)
- [ ] Baseline kernel version and hardware details are documented above
- [ ] eBPF and BTF support have been verified
- [ ] Lab machine is isolated from production or critical systems
- [ ] This document reflects actual lab configuration

## Future Evolution

### Phase 1 Preparation

Once Phase 0 is verified, prepare for Phase 1 (Linux Fundamentals):
- [ ] Install strace for syscall tracing
- [ ] Verify /proc filesystem accessibility
- [ ] Document any kernel modules or security restrictions
- [ ] Plan first syscall tracing experiment

### Documentation Updates

As the lab evolves:
- Update this document with installed tools and versions
- Record kernel parameters changed for any experiment
- Document container runtime selection and configuration
- Track eBPF program development environment setup

### Archive Snapshots

Consider capturing lab machine state at key milestones:
- After kernel verification
- After each major tool installation
- Before and after large experiments
- For baseline comparison during troubleshooting

---

**Last Updated:** Phase 0 initialization  
**Lab Status:** Foundation established, awaiting Phase 1 preparation
