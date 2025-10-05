# fc42-gfx1151-meswa-kernel

Temporary Fedora 42 kernel rebuilt with a cherry-picked upstream fix for AMD Strix Halo gfx1151 stability.

* Commit: [https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17](https://github.com/torvalds/linux/commit/1fb710793ce2619223adffaf981b1ff13cd48f17)
* Tag used: `linux-6.16.10-200.gfx1151_meswa.fc42`
* More context: [https://github.com/kyuz0/triton_gfx1151_crashes](https://github.com/kyuz0/triton_gfx1151_crashes)

## IMPORTANT: Secure Boot

Disable Secure Boot in firmware **before installing**. If you leave it on and you do not sign these RPMs with a key enrolled in MOK, the kernel will be refused at boot and the system will fall back to another entry or fail to boot this kernel.

## Install

```bash
git clone https://github.com/kyuz0/fc42-gfx1151-meswa-kernel.git
cd fc42-gfx1151-meswa-kernel

# install only non-debug RPMs, install all local subpackages together
sudo dnf install ./x86_64/*.rpm \
  --exclude 'kernel-debug*' \
  --exclude '*debuginfo*'
```

## Verify

```bash
# check default boot target
sudo grubby --default-kernel

# to set this kernel as default if needed
sudo grubby --set-default /boot/vmlinuz-6.16.10-200.gfx1151_meswa.fc42.x86_64
```

## Revert / remove

```bash
# list installed kernels from this build
rpm -qa | grep '^kernel-.*gfx1151_meswa'

# remove this build (keep others)
sudo dnf remove 'kernel*-6.16.10-200.gfx1151_meswa.fc42*'

# or roll back the last install transaction
sudo dnf history
sudo dnf history undo <ID>
```

## Notes

* Built for Fedora 42 x86_64.
* These are locally built RPMs with a single upstream commit cherry-picked to address gfx1151 stability under workloads like llama.cpp and PyTorch.
* Do not mix these subpackages with repo ones. Install all RPMs from this repo in one command to avoid pulling Fedora’s kernel pieces.
