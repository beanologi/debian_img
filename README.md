# Debian Image Build Tools

This project is still in the drafting and development phase. 

Things can and will deviate from the original plan, it is inevitable.

## Goals

- Be able to produce an identical image, every time, without using the slow Debian snapshot repo.
- Support UEFI and Master Boot Record boot methods in the same image.
- Use Docker containers to isolate the build process.
- Only use higher system privileges out of necessity.
- Image can be customized and preloaded with packages and configs.
- The process can be broken up into smaller chunks.


## Notes
  - Docker privileged mode is only required for debootstrap, it doesn't work right if we don't.
  - We use an official Debian DVD ISO as the repo for bootstrapping to ensure consistency.
    - However, If you wish to customize the resulting image with packages beyond what the DVD can provide, you should instead supply the extended 16GB ISO that can be downloaded with jigdo.
  - There should be a postinstall script that sets up the system with an optional root password, sudo administrator user, timezone, locale, fstab, apt repo, etc.
  - There should also be a firstboot script that optionally can run whatever you want on first boot.
  - The bootstrapping container outputs a tarball of the rootfs so the special device node files can be contained without demanding special privileges for the rest of the process.
  - Disk image and filesystem manipulation is done using guestfish and not with system-level mounting. This means we can separate these tasks into a completely unprivileged part of the process.
  - The bootloader of choice is GRUB2 for UEFI and MBR.
  - The resultant image is a *raw disk image (.img)* - not an ISO file! That means you can write it to a USB flashdrive and any data you write, will persist after a reboot because it is not readonly. The best comparison is to think of the Raspbian disk images for Raspberry Pi Computers. 
