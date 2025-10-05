# fc42-gfx1151-meswa-kernel

Temporary Fedora 42 kernel rebuilt with a cherry-picked upstream fix for AMD Strix Halo **gfx1151** stability.

* Commit: [https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17](https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17)
* Tag used: `linux-6.16.10-200.gfx1151_meswa.fc42`
* More context: [https://github.com/kyuz0/triton_gfx1151_crashes](https://github.com/kyuz0/triton_gfx1151_crashes)

This build integrates that fix ahead of the 6.18 release to improve stability under GPU-heavy workloads such as **llama.cpp**, **PyTorch**, and other compute loads.

---

## ⚠️ IMPORTANT: Secure Boot

Before installing, **disable Secure Boot** in your firmware (BIOS/UEFI).
Unsigned custom kernels will be rejected by Secure Boot — if it’s left enabled, the system will either:

* Refuse to boot this kernel, or
* Silently fall back to another installed kernel.

---

## Install

Download the prebuilt RPMs from the [Releases page](https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/tag/6.16.10-200.gfx1151_meswa.fc42)
or use the CLI commands below.

```bash
mkdir fc42-gfx1151-meswa-kernel && cd fc42-gfx1151-meswa-kernel

# download required RPMs
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-core-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-modules-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-modules-core-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-modules-extra-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-uki-virt-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
wget -q https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases/download/6.16.10-200.gfx1151_meswa.fc42/kernel-uki-virt-addons-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
```

Then install all downloaded RPMs together:

```bash
sudo dnf install ./*.rpm
```

If you also need kernel headers for DKMS or external module builds:

```bash
sudo dnf install kernel-devel-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
```

---

## Verify

```bash
uname -r
# expected: 6.16.10-200.gfx1151_meswa.fc42.x86_64

# check the current default boot entry
sudo grubby --default-kernel

# set this kernel as default if needed
sudo grubby --set-default /boot/vmlinuz-6.16.10-200.gfx1151_meswa.fc42.x86_64
```

---

## Revert / Remove

```bash
# list installed kernels from this build
rpm -qa | grep '^kernel-.*gfx1151_meswa'

# remove this build (keep others)
sudo dnf remove 'kernel*-6.16.10-200.gfx1151_meswa.fc42*'

# or roll back the last transaction
sudo dnf history
sudo dnf history undo <ID>
```

---

## Notes

* Built for **Fedora 42 x86_64**.
* Contains a single upstream commit addressing **gfx1151** GPU instability.
* Install all RPMs together to avoid mixing with Fedora’s stock kernel subpackages.
* Reboot after installation.
* For further technical discussion or issue tracking, see [triton_gfx1151_crashes](https://github.com/kyuz0/triton_gfx1151_crashes).

