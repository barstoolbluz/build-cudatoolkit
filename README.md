# CUDA Toolkit 13.0 - Flox Package Build

This repository builds and packages NVIDIA CUDA Toolkit 13.0 for distribution via Flox.

## What This Builds

CUDA Toolkit 13.0.88 from nixpkgs, including:
- CUDA compiler (nvcc)
- CUDA debugger (cuda-gdb)
- Core libraries (cublas, cufft, cusparse, cusolver, curand)
- CUDA runtime (cudart)
- Development tools (nvprune, nvdisasm, cuobjdump)

## Platform Support

- x86_64-linux
- aarch64-linux

The Nix expression automatically fetches and patches the correct binaries for your platform.

## Building

```bash
flox build cudatoolkit-13_0
```

This creates a `result-cudatoolkit-13_0` symlink to the built package in `/nix/store`.

## Testing the Build

```bash
./result-cudatoolkit-13_0/bin/nvcc --version
```

## Publishing

After committing and pushing to a git remote:

```bash
flox publish cudatoolkit-13_0
# Or to publish to an organization:
# flox publish -o <org-name> cudatoolkit-13_0
```

## Installation

After publishing, users can install with:

```bash
flox install <username>/cudatoolkit-13_0
# Or: flox install <org-name>/cudatoolkit-13_0
```

## Technical Details

This uses a Nix expression (`.flox/pkgs/cudatoolkit-13_0.nix`) that references `cudaPackages_13.cudatoolkit` from nixpkgs. The nixpkgs CUDA infrastructure handles downloading NVIDIA redistributable binaries and patching them for Nix.
