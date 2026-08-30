# Tetragon Threat Detection Lab

A structured, hands-on research repository documenting the journey into Linux runtime security, containers, Kubernetes, eBPF, Cilium Tetragon, and detection engineering.

## Objective

This project explores how attacker behaviour becomes observable at the Linux kernel and runtime layers, and how those observations can be translated into Tetragon runtime detections and enforcement policies.

## Learning Path

The project follows a progressive technical path:

**Linux → Containers → Kubernetes → eBPF → Tetragon → Detection Engineering → CVE/Threat Research**

## Approach

This is a hands-on experimentation and research repository. We prioritize:
- **Reproducibility**: Every step is documented and repeatable
- **Reasoning**: Clear explanations of *why* something behaves as it does
- **Testing**: Validation of assumptions through controlled experiments
- **False-positive analysis**: Understanding detection accuracy and tuning

## Current Status

**Phase 0: Lab Foundation**

The lab environment is initialized with documentation of the architecture, environment setup, and design decisions. As work progresses, this repository will grow with:
- Phase 1: Linux processes, syscalls, /proc and strace fundamentals
- Future phases: Container runtime, Kubernetes, eBPF programs, Tetragon policies, and detection frameworks

## Repository Structure

```
tetragon-threat-detection-lab/
├── README.md                      # Project overview
├── LICENSE                        # Apache 2.0
├── .gitignore                     # Version control exclusions
├── docs/
│   └── lab-architecture.md        # Architecture and planned evolution
├── lab/
│   └── phase-00/
│       └── environment.md         # Lab environment documentation
└── assets/
    └── diagrams/                  # Mermaid and other diagrams
```

## Important Distinction

This repository documents work performed in a dedicated Ubuntu Server 24.04 LTS lab environment (separate physical machine). Documentation is maintained from a development workstation; the lab environment itself is where experiments and observations occur.

## License

This project is licensed under the Apache License 2.0. See the LICENSE file for details.