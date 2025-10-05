# fc42-gfx1151-meswa-kernel

Temporary Fedora 42 kernel rebuilt with a cherry-picked upstream fix for AMD Strix Halo gfx1151 stability.

* Commit: [https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17](https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17)
* Tag used: `linux-6.16.10-200.gfx1151_meswa.fc42`
* More context: [https://github.com/kyuz0/triton_gfx1151_crashes](https://github.com/kyuz0/triton_gfx1151_crashes)

---

## ⚠️ IMPORTANT: Secure Boot

Disable **Secure Boot** in your firmware **before installing**.
Unsigned custom kernels will be rejected by Secure Boot.
If left enabled, the system will either refuse to boot this kernel or silently fall back to a previous one.

---

## Install

Download the prebuilt RPMs from the [Releases page](https://github.com/kyuz0/fc42-gfx1151-meswa-kernel/releases).

Then install them all at once:

```bash
# from the directory where you downloaded the RPMs
sudo dnf install \
  ./kernel-{core,modules,modules-core,modules-extra,uki-virt,uki-virt-addons}-6.16.10-200.gfx1151_meswa.fc42.x86_64.rpm
```

(Optionally install `kernel-devel` if you need headers for DKMS or external modules.)

---

## Verify

```bash
uname -r
# expected: 6.16.10-200.gfx1151_meswa.fc42.x86_64

# check current default kernel
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
* Contains one upstream commit that improves gfx1151 GPU stability under heavy compute workloads (llama.cpp, PyTorch, etc.).
* Install all provided RPMs together to avoid mixing with Fedora’s kernel subpackages.

