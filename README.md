# Linux-Patch

Custom patch collection for the Linux kernel, focused on the `linux-6.17.y` series.
This repository organizes patches into logical groups (for example `base/`, `sched/`)
to make applying, updating, and sharing kernel tweaks easier.

## Repository layout

- `base/`
	- `0001-Update-Kconfig.cpu-to-Fix-Build-Error.patch` – Fixes a configuration
		issue in `Kconfig.cpu` that can cause build failures.
	- `0002-Makefile-O3-Optimization.patch` – Adjusts kernel `Makefile` options
		to enable `-O3` compilation optimization.
- `sched/`
	- `0001-prjc_v6.17-r1.patch` – Scheduler-related patch (PRJC) for `v6.17-r1`.
	- `0001-prjc-LFBMQ_v6.17-r1.patch` – Scheduler patch for PRJC LFBMQ variant
		on `v6.17-r1`.
	- `0002-Fix-Patch-Failed-in-syscall.c.patch` – Fixes a previously failing
		patch application in `syscall.c`.
	- `0003-Avoid-sysctl.c-Errors.patch` – Adjusts `sysctl.c` to avoid
		build/runtime errors when patching.

> Filenames describe their purpose at a high level. For exact changes, open the
> patch and read the diff.

## Requirements

You should already have:

- A Linux kernel source tree (matching or close to `linux-6.17.y`).
- Standard development tools:
	- `git`
	- `patch` or `git apply`
	- `make`, `gcc`, etc. (for building the kernel)

Example kernel source directory:

```text
~/src/linux-6.17.y
```

## How to apply the patches

The following examples assume your kernel tree is in `~/src/linux-6.17.y` and
this repository is in `/home/hakuu/Documents/GitHub/Linux-Patch`.

1. Change into the kernel tree:

	 ```bash
	 cd ~/src/linux-6.17.y
	 ```

2. Apply base patches:

	 ```bash
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/base/0001-Update-Kconfig.cpu-to-Fix-Build-Error.patch
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/base/0002-Makefile-O3-Optimization.patch
	 ```

3. Apply scheduler patches:

	 ```bash
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/sched/0001-prjc_v6.17-r1.patch
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/sched/0001-prjc-LFBMQ_v6.17-r1.patch
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/sched/0002-Fix-Patch-Failed-in-syscall.c.patch
	 patch -p1 < /home/hakuu/Documents/GitHub/Linux-Patch/sched/0003-Avoid-sysctl.c-Errors.patch
	 ```

4. Build the kernel (example):

	 ```bash
	 make olddefconfig
	 make -j"$(nproc)"
	 ```

Adjust the patch order or selection depending on your configuration and needs.

### Using `git apply`

If your kernel source is a Git repo, you can also use:

```bash
git apply /home/hakuu/Documents/GitHub/Linux-Patch/base/0001-Update-Kconfig.cpu-to-Fix-Build-Error.patch
```

Repeat for each patch.

## Recommended workflow

- Maintain a branch (for example `linux-6.17.y`).
- Apply patches from this repo on top of upstream.
- When updating to a newer `6.17.y` point release, rebase or re-apply the
	patches and resolve conflicts.

Example workflow:

```bash
git checkout -b linux-6.17.y-custom origin/linux-6.17.y
# apply patches as above
git commit -am "Apply custom Linux-Patch patchset"
```

## Contributing

If you want to extend this patch collection:

1. Create your patch against a clean `linux-6.17.y` tree.
2. Name it descriptively (keep the numeric prefix ordering).
3. Place it in the appropriate subdirectory (`base/`, `sched/`, or a new one).
4. Optionally open an issue or pull request with:
	 - Kernel version and config (if relevant).
	 - Short description of the change.
	 - Any performance/behavior notes.

## License

This repository is licensed under the terms described in `LICENSE`.
Individual patches may be derived from the upstream Linux kernel and therefore
fall under the kernel's license (GPLv2).

 
