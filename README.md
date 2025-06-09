# CTF_Writeups_2025

## Capture the Flags

| CTF | Dates | 
| --- | ----- | 
| TU CTF | 1/24 - 1/26 | 
| LA CTF | 2/6 - 2/8 |
| N0ps CTF | 5/31 - 6/1 |
| TJ CTF | 6/6 - 6/8 |
| DoD Cyber Sentinel Challenge | 6/14 |

## Setting up my enivornment

I have an old computer that I use only for CTF's and practice. Instead of using a virtual machine of Kali, I installed Kali as my main operating system on that computer. I followed the instructions from [Kali's Hard Disk installation docs](https://www.kali.org/docs/installation/hard-disk-install/)

1. Since I start from a Windows device, and chose to boot from a USB drive, I followed [Kali's live USB install docs](https://www.kali.org/docs/usb/live-usb-install-with-windows/)
  - Download a [live image](https://www.kali.org/get-kali/#kali-live) to boot from a USB stick
  - Download Etcher from [their website](https://etcher.balena.io/)
  - Use Etcher to flash the live image to the USB drive
2. Fix bios settings to allow `Legacy` and `disable secure boot`
3. Boot from live USB and select installer in GRUB menu
4. Follow instructions and profit
