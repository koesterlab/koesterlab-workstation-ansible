# prerequisites

## Install Windows

When starting your machine for the first time you can simply set it up with the regular windows installer that should show up by default.

**Turn off Windows device encryption**: Under Windows, make sure to turn off `Device encryption` (`Settings -> Privacy and Security -> Device encryption`), so that Fedora can be installed.
This will take a moment, showing `Decryption in progress`.

## Install Fedora

Once all the Windows tasks are done, you can install fedora.

1. Create a bootable hard drive (for example on a USB stick with at least 2.5 GB space) using the [fedora media writer](https://fedoraproject.org/workstation/download).
2. Plug the USB drive into the computer and restart it, repeatedly pressing `F12`.
3. In the boot menu that you get, select the USB drive with the Fedora installer.
4. In the following text-based menu, select to `Test this media & start Fedora-Workstation-Live`.
5. If the media test fails, ensure that previous to bootable hard drive creating you formatted that hard drive to GPT partitioning and don't start Windows with the Fedora USB drive connected.
6. Select installation.
7. In the step `Install Destination`, select `Advanced Custom (Blivet-GUI)` under `Storage Configuration`.
8. In the following partitioning GUI, create a setup like this:
   1. **Existing windows partition: shrink to 80 GB**:
    [64 GB is the recommended minimum storage for Windows 11](https://www.microsoft.com/en-us/windows/windows-11-specifications), but the plain Windows installation already uses 54 GB, and it probably makes sense to have more than 10 GB for Updates or extra Software, even when not really using Windows much.
   2. **In the resulting free space, create new partitions**:
    The [`Default Disk Layout`](https://docs.fedoraproject.org/en-US/workstation-docs/disk-config/#_default_disk_layout) is a good guideline, with sizes based on the [Fedora partitioning recommendations](https://docs.fedoraproject.org/en-US/fedora/f36/install-guide/install/Installing_Using_Anaconda/#sect-installation-gui-manual-partitioning-recommended).
    For extra data security, encrypt the `/` (root) and `/home` partitions (use a password you can type manually, you will be asked at every startup). A working setup should be:
      1. **mount point `/boot` (bootloader)**: Format `ext4`, size at least `1 GB` (maybe add a little extra space, so you don't have to re-partition later).
      2. **mount point `/boot/efi` (kernels etc.)**: Format `efi`, size at least `500 MB` (maybe add a little extra space, so you don't have to re-partition later).
      3. **mount point `/` (root)**: Format `ext4`, size at least `25 GB` (maybe add a little extra space, so you don't have to re-partition later).
      4. **mount point `/home`**: Format `ext4`, use all the remaining space.
9. Confirm the partitioning scheme and start the actual installation.
10. Upon completion, shut down the machine, remove the USB drive and start back up.

If the installation was successfull, you can now select Fedora (and Windows) from the new GRUB menu.

## safe-guard against Windows Updates kicking GRUB

We have seen instances, where Windows updates lead to losing the GRUB menu.
This seems to be known, and the solution is to [make the Windows Boot Manager the first entry in the EFI Boot Order, while also deactivating it](https://unix.stackexchange.com/a/547371).
This [should be possible via `efibootmgr`](https://unix.stackexchange.com/a/547371), however its version `18` (current at the time of writing) [contains a bug that is prevents this](https://github.com/rhboot/efibootmgr/issues/186).
Thus, in the future we can probably automate this via ansible, but for now you will have to do this via the system firmware:

1. Start your computer while repeatedly pressing `F12`.
2. In the firmware, go to `Boot`.
3. Select `New Boot Deive Priority` and switch it to `First`.
4. Select `EFI` Boot Order`, deactivate the `Windows Boot Manager` and move it to the top of the Boot Order (with `F6`).
5. `Save and Exit` with `F10`.

You can continue setting up Fedora by installing ansible with `sudo dnf install ansible` and following the guide in the [README.md](README.md) file.
