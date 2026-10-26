# KernelSU removal log

Source: AGNi_BASE-VILI_AGNI (Linux 5.4.283, vili branch)

## Files removed
- `drivers/kernelsu` (symlink to the `KernelSU/` submodule; the submodule was empty in the upload)
- `KernelSU/` (empty directory)
- `.gitmodules` (only contained the KernelSU submodule entry)

## Build wiring removed
- `drivers/Kconfig`: `source "drivers/kernelsu/Kconfig"`
- `drivers/Makefile`: `obj-$(CONFIG_KSU) += kernelsu/`
- `arch/arm64/configs/agni_vili_defconfig`: `CONFIG_KSU=y`, `# CONFIG_KSU_DEBUG is not set`, and the `# KernelSU` / `# end of KernelSU` comments

## Core kernel hooks removed
- `fs/exec.c`: `ksu_handle_execveat` (3 sites) and `ksu_handle_execveat_sucompat`, plus `ksu_execveat_hook` declarations and the guarded call block in `do_execveat_common`
- `fs/stat.c`: `ksu_handle_stat` declaration and call in `vfs_statx`
- `fs/read_write.c`: `ksu_handle_vfs_read` declaration and call in `vfs_read`
- `fs/open.c`: `ksu_handle_faccessat` declaration and call in `do_faccessat`
- `drivers/input/input.c`: `ksu_input_hook` and `ksu_handle_input_handle_event` declarations and the guarded call in `input_handle_event`

## ABI symbol list
- `android/abi_gki_aarch64_agni.xml`: removed the `ksu_kallsyms_lookup_name` entry (the XML still parses)

## Not changed
- MIPS `KSU_USER` / `KSU_KERNEL` register macros (unrelated CPU status-register names)
- `kernel/sys.c` `ksuid` variable and `tools/power/pm-graph` `ksuscalls` (unrelated names)
- Everything else, including the vili DTS, `vendor/vili_QGKI.config`, and `agni_vili_defconfig` apart from the KSU lines above

## Not verified
- The tree has not been compiled. Build it and send any errors.
