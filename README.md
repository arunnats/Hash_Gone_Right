# Hash Gone Right 

**B.Tech Final Year Thesis Project**  
**NIT Calicut, 2024-25**

**Authors:** Arun Natarajan, Hafeez Muhammed  
**Advisor:** Dr. Vinod Pathari

## Overview

This project extends the work from "Hash Gone Bad" (USENIX Security'23), focusing on the ProVerif implementation of computation functions for modeling hash function weaknesses in cryptographic protocol verification.

## Goals

- Analyze the ProVerif-side implementation of computation functions
- Study hash modeling techniques in symbolic verification
- Propose improvements to the existing framework
- Explore additional hash-based protocol vulnerabilities
- Implement and evaluate axioms and unification algorithms    that improve ProVerif’s resolution for hash-based protocols.

## Repository Structure

```
├── Archives/         # Archived files and older artifacts
├── Docker/           # Docker resources and reference notes
├── MDH_construct/    # Main ProVerif framework, protocols, libs, and benchmarks
├── Protocol Models/  # Original protocol model collection
├── diff-check/       # Scripts for generating and filtering diffs
└── README.md
```

## MDH_construct Structure

The `MDH_construct/` directory is the core workspace used for protocol verification and benchmarking with both current and legacy hash-model libraries.

```
MDH_construct/
├── Makefile
├── Protocols/
├── Testing/
├── libs/
└── README.md
```

- **Makefile**: Central entry point for ProVerif runs. It selects protocol families (`ike`, `ike_s`, `sigma`, `universal`, `protosuite`), hash modes (`assoc`, `no_collision`, `collision`), and library set (`LIB_SET=current|legacy`).
- **Protocols/**: Contains all protocol specifications used by the benchmark pipeline, including the adapted protocol suite and protocol-specific declaration/model files.
- **Testing/**: Contains benchmark wrappers, per-protocol `benchmark.res` summaries, and local per-protocol log directories for reproducible runs.
- **libs/**: Contains hash-model libraries split by version:
  - `libs/current/` for the improved implementation.
  - `libs/legacy/` for the baseline/original implementation used for comparison.
- **README.md**: Local documentation for the MDH workspace and usage notes specific to this subproject.


## Setup and Run Guide

This repository (**Hash_Gone_Right**) contains the implementation of **Jaffar's algorithm**, plus **resolution-level merge axioms** and **attacker-head variable axioms**.

### 1. Clone the Repository

```
git clone https://github.com/Hafeez-hm/Hash_Gone_Right.git
cd Hash_Gone_Right
```

### 2. Pull the Docker Image

```
docker pull hafeez2003/proverif-jaffar:latest
```

### 3. Run the Docker Container

```
docker run -it -v ~/projects/Fast_and_FuriHash:/root hafeez2003/proverif-jaffar:latest /bin/bash
```

### 4. Enter the Main Verification Workspace

Inside the container, go to:

```
cd /root/MDH_construct
```

### 5. Understand the Core MDH Directories

- `libs/`: Contains the hash-model libraries.
  - `libs/current/` contains the improved implementation.
  - `libs/legacy/` contains the baseline implementation for comparison.
- `Protocols/`: Contains all modeled protocol files used by verification runs.
- `Testing/`: Contains per-protocol benchmark folders, each with a `benchmark.sh` runner.

### 6. Run Benchmarks

Each protocol under `Testing/` has its own `benchmark.sh`.
The script runs the protocol against both `current` and `legacy` libraries, writes detailed logs, and generates a summary `.res` file.

Example (Simplified IKE):

```
cd /root/MDH_construct/Testing/simplified_ikeV2_HF_EC
bash benchmark.sh
```

### 7. View Results

- Summary output: `benchmark.res` in that protocol's testing folder.
- Detailed logs: `logs/current/` and `logs/legacy/` in the same folder.

### 8. Run a Single Protocol Directly with Make

From `/root/MDH_construct`:

```
make ike=1 LIB_SET=current
make ike=1 LIB_SET=legacy
```

Available selectors include `ike`, `ike_s`, `sigma`, `universal`, and `protosuite`.

### 9. Main Axiom Library Files

The main axioms are implemented in:

- `libs/current/hash_no_collision.pvl`
- `libs/current/hash_collision.pvl`
- `libs/current/assoc_no_collision.pvl`

Legacy baselines are available under the same filenames in `libs/legacy/`.

## Resources

- **Docker Image:** [hafeez2003/proverif-jaffar](https://hub.docker.com/r/hafeez2003/proverif-jaffar)
- **DeepWiki:** [deepwiki.com/arunnats/proverif-compfun/](https://deepwiki.com/arunnats/proverif-compfun/)
- **Original Paper:** [Hash Gone Bad (USENIX Security'23)](https://www.usenix.org/conference/usenixsecurity23/presentation/cheval)
- **Original Repository:** [charlie-j/symbolic-hash-models](https://github.com/charlie-j/symbolic-hash-models)
- **ProVerif Documentation:** [bblanche.gitlabpages.inria.fr/proverif](https://bblanche.gitlabpages.inria.fr/proverif/)

## License

This project builds upon ProVerif 2.03 (INRIA, CNRS 2000-2021) and the modifications from "Hash Gone Bad" (2023).

## Contact

- Arun Natarajan - [GitHub](https://github.com/arunnats)
- Hafeez Muhammed - [GitHub](https://github.com/Hafeez-hm)
