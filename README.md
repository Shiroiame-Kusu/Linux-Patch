# Linux-Patch

Custom patch collection for the Linux kernel, focused on the `linux-6.18.y` series.
Patches are grouped so you can apply only what you need.

## Repository layout

- `base/`
	- `0001-O3-Optimization.patch` – Enables `-O3` optimization in the kernel build.
	- `0002-Fix-Native-Build-Error.patch` – Fixes a native build issue in the toolchain/config.
- `sched/`
	- `0003-Patch-sysctl.c-Table-to-Prevent-Register-Failed.patch` – Adjusts `sysctl.c` table handling to avoid registration failures.
	- `BMQ/`
		- `0001-prjc_v6.18-r0.patch` – BMQ scheduler patch for the 6.18 series.
		- `0002-syscall.c-do_sched_yield-Manually-Fix.patch` – Manual fix for `do_sched_yield` in `syscall.c` for BMQ.

> Filenames describe intent at a high level; read each patch for details.

## Requirements

- Linux kernel source tree matching (or close to) `linux-6.18.y`.
- Standard tools: `git`, `patch` (or `git apply`), `make`, `gcc`, etc.

Example kernel source path:

```text
~/src/linux-6.18.y
```

## How to apply the patches

Set paths (adjust to your setup):

```bash
export KERNEL_SRC=~/src/linux-6.18.y
export PATCH_ROOT=${PATCH_ROOT:-~/Documents/GitHub/Linux-Patch}
cd "$KERNEL_SRC"
```

Apply base patches:

```bash
patch -p1 < "$PATCH_ROOT/base/0001-O3-Optimization.patch"
patch -p1 < "$PATCH_ROOT/base/0002-Fix-Native-Build-Error.patch"
```

Apply scheduler patch:

```bash
patch -p1 < "$PATCH_ROOT/sched/0003-Patch-sysctl.c-Table-to-Prevent-Register-Failed.patch"
```

Apply BMQ scheduler patches (optional, if you use BMQ):

```bash
patch -p1 < "$PATCH_ROOT/sched/BMQ/0001-prjc_v6.18-r0.patch"
patch -p1 < "$PATCH_ROOT/sched/BMQ/0002-syscall.c-do_sched_yield-Manually-Fix.patch"
```

Build the kernel (example):

```bash
make olddefconfig
make -j"$(nproc)"
```

### Using `git apply`

If your kernel source is a Git repo, you can also use:

```bash
git apply "$PATCH_ROOT/base/0001-O3-Optimization.patch"
```

Repeat for each patch you need.

## Recommended workflow

- Work on a branch (for example `linux-6.18.y-custom`).
- Apply patches on top of upstream `linux-6.18.y`.
- When rebasing to newer `6.18.y` point releases, re-apply patches and resolve conflicts.

Example flow:

```bash
git checkout -b linux-6.18.y-custom origin/linux-6.18.y
# apply patches
git commit -am "Apply custom Linux-Patch patchset"
```

## Contributing

1. Create patches against a clean `linux-6.18.y` tree.
2. Use clear names with numeric prefixes to keep ordering obvious.
3. Place patches in the right folder (`base/`, `sched/`, `sched/BMQ/`, or a new one).
4. If you share changes, include kernel version/config, a short description, and behavior/performance notes.

## License

This repository is licensed under the terms in `LICENSE`.
Patches derived from the upstream Linux kernel follow the kernel's license (GPLv2).

