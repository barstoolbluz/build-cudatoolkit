# CUDA Toolkit 13.0 - Flox Package Build

This repository builds and packages NVIDIA CUDA Toolkit 13.0 components for distribution via Flox.

## What This Builds

### cudatoolkit-13_0 (v13.0.88)
CUDA Toolkit core package including:
- CUDA compiler (nvcc)
- CUDA debugger (cuda-gdb)
- Core libraries (cublas 13.1.0.3, cufft 12.0.0.61, cusparse 12.6.3.3, cusolver 12.0.4.66, curand 10.4.0.35)
- CUDA runtime (cudart 13.0.96)
- Development tools (nvprune, nvdisasm, cuobjdump)

### cudnn-13_0 (v9.13.0.50)
GPU-accelerated library of primitives for deep neural networks

### nccl-13_0 (v2.28.7-1)
Multi-GPU and multi-node collective communication primitives for NVIDIA GPUs

### libcusparse_lt-13_0 (v0.8.1.1)
High-performance library for structured sparse matrix operations

## Platform Support

- x86_64-linux
- aarch64-linux

The Nix expression automatically fetches and patches the correct binaries for your platform.

## Building

Build individual packages:
```bash
flox build cudatoolkit-13_0
flox build cudnn-13_0
flox build nccl-13_0
flox build libcusparse_lt-13_0
```

Or build all packages at once:
```bash
flox build
```

This creates `result-<package>-13_0` symlinks to the built packages in `/nix/store`.

## Testing the Build

```bash
./result-cudatoolkit-13_0/bin/nvcc --version
ls result-cudnn-13_0-lib/lib/
ls result-nccl-13_0/lib/
ls result-libcusparse_lt-13_0-lib/lib/
```

## Publishing

After committing and pushing to a git remote:

```bash
# Publish individual packages
flox publish cudatoolkit-13_0
flox publish cudnn-13_0
flox publish nccl-13_0
flox publish libcusparse_lt-13_0

# Or publish all at once
flox publish

# To publish to an organization:
# flox publish -o <org-name>
```

## Installation

After publishing, users can install with:

```bash
flox install <username>/cudatoolkit-13_0
flox install <username>/cudnn-13_0
flox install <username>/nccl-13_0
flox install <username>/libcusparse_lt-13_0

# Or install all at once:
# flox install <username>/cudatoolkit-13_0 <username>/cudnn-13_0 <username>/nccl-13_0 <username>/libcusparse_lt-13_0
```

## Technical Details

Each package uses a simple Nix expression in `.flox/pkgs/` that references the corresponding package from nixpkgs' `cudaPackages_13` set:

- `cudatoolkit-13_0.nix` → `cudaPackages_13.cudatoolkit`
- `cudnn-13_0.nix` → `cudaPackages_13.cudnn`
- `nccl-13_0.nix` → `cudaPackages_13.nccl`
- `libcusparse_lt-13_0.nix` → `cudaPackages_13.libcusparse_lt`

The nixpkgs CUDA infrastructure handles:
- Downloading NVIDIA redistributable binaries for the target platform
- Patching binaries for Nix compatibility (RPATH fixes, etc.)
- Managing multi-output derivations (lib, dev, include, static)
