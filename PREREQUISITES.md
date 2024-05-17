# prerequisites

When starting your machine for the first time you can simply set it up with the regular windows installer that should show up by default.
Under Windows, make sure to turn off `Device encryption` (`Settings -> Privacy and Security -> Device encryption`), so that Fedora can be installed.
This will take a moment, showing `Decryption in progress`.
Once that is done, install fedora by creating a bootable hard drive (for example a usb stick) using the [fedora media writer](https://fedoraproject.org/workstation/download).

If the installation was successfull you can continue setting up your machine by installing ansible with `sudo dnf install ansible` and following the guide in the [README.md](README.md) file.
