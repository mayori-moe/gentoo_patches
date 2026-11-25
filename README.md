# Gentoo Patches
Some tweaks for using musl-libc based Gentoo.

Basically, there is:
- 1. Personal patches for some packages inside `/etc/portage/patches`
- 2. Possibly custom kernel config

Some of the patches are musl-libc only; some are just personal tweaks not appropriate for upstream.

You can try these patches if you are also trying a musl based installation, and you don't want to build a personal overlay for some simple optimization or bug fix.

- `nftables/ethhdr.patch`:
See [Gentoo Bugs 945470](https://bugs.gentoo.org/945470).

- `cachyos-kernel/iommu.patch`:
Bypassing limitations on kernel to passthrough RTX 50 series GPUs.

- `cachyos-kernel/0001*.patch`:
See [this work item](https://gitlab.freedesktop.org/drm/amd/-/work_items/5339) for certain AMD iGPUs, not merged into mainline yet, with some changes.
- `cachyos-kernel/fake_rdtsc.patch.disable`:
See [RDTSC-KVM-Handler](https://github.com/WCharacter/RDTSC-KVM-Handler); anti-detection for some Windows anticheat running inside qemu-kvm, with some changes. Remove `.disable` suffix to use.

  These kernel patches should also apply to `sys-kernel/gentoo-kernel`, but not tested.

- `musl`:
    - `Add-rpmalloc-for-musl.patch`: Patch musl with [`rpmalloc`](https://github.com/mjansson/rpmalloc). Adapted from [this patch](https://github.com/12101111/overlay/blob/master/sys-libs/musl/files/Add-rpmalloc-for-musl.patch); with the help of Gemini Neon, update rpmalloc version 1.4.5 to develop.
  
  These patches can improve system performance when you are using musl. The default `mallocng` can slow down performance a lot; see [this blog](https://nickb.dev/blog/default-musl-allocator-considered-harmful-to-performance). `rpmalloc` is small enough for a single patch but it has a good performance.

- `rtkit/sched_getscheduler_sched_setscheduler.patch`:
See [Gentoo Bugs 980022](https://bugs.gentoo.org/980022) and [this patch](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/community/rtkit/sched_getscheduler_sched_setscheduler.patch) from Alpine Linux for fixing some issues under musl.

- `kde-plasma/layer-shell-qt/layer-shell-qt-llvm-abi.patch && dev-qt/qtbase/qtbase-fix-llvm-abi.patch`
See [Gentoo Bugs 923292](https://bugs.gentoo.org/923292). Patches adapted from FreeBSD patches, according to [this comment](https://bugs.kde.org/show_bug.cgi?id=479679#c28) of KDE Bugs 479679. If you don't want to use this patch, provide an `env` file for `llvm-runtimes/libcxx` contains `MYCMAKEARGS="-DLIBCXX_TYPEINFO_COMPARISON_IMPLEMENTATION=2"` then rebuild all packages.

- `net-analyzer/openbsd-netcat/netcat-define-IPTOS_DSCP_VA-for-musl.patch`
See [Gentoo Bugs 981134](https://bugs.gentoo.org/981134). Patch for the MACRO not defined in musl.

If you are having problems when using a musl profile, searching about related packages or patches on Gentoo Bugs/Void Linux/Alpine Linux/Chimera Linux is helpful, and you can find lots of useful patches there.
