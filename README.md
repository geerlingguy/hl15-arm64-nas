# Arm NAS

Ansible playbook to configure my Arm NASes:

  - [HL15 with Ampere Altra](https://www.youtube.com/watch?v=Hz5k5WgTkcc)
  - [Raspberry Pi 5 SATA NAS](https://www.youtube.com/watch?v=l30sADfDiM8)

This playbook assumes you're running Ubuntu 20.04 Server LTS and/or Debian 12.

## Hardware

### Primary NAS - 45Drives HL15

<p align="center"><img alt="45Homelab HL15 with Jeff Geerling hardware" src="/resources/hl15-hardware.jpeg" height="auto" width="600"></p>

The current iteration of the HL15 I'm running contains the following hardware:

  - (Motherboard) [ASRock Rack ALTRAD8UD-1L2T](https://reurl.cc/qrnXNp) ([specs](https://reurl.cc/67jk0V))
  - (Case) [45Homelab HL15 + backplane + PSU](https://store.45homelab.com/configure/hl15)
  - (PSU) [Corsair RM750e](https://amzn.to/3OyDQ79)
  - (RAM) [8x Samsung 16GB 1Rx4 ECC RDIMM M393A2K40DB3-CWE PC25600](https://amzn.to/49lCtkb)
  - (NVMe) [Kioxia XG8 2TB NVMe SSD](https://amzn.to/3Uzag5d)
  - (CPU) [Ampere Altra Q32-17](https://amperecomputing.com/briefs/ampere-altra-family-product-brief)
  - (SSDs) [4x Samsung 8TB 870 QVO 2.5" SATA](https://amzn.to/3OylbZk)
  - (HDDs) [6x Seagate EXOS 20TB SATA HDD](https://amzn.to/3OA2CDM)
  - (HBA) [Broadcom MegaRAID 9405W-16i](https://amzn.to/3srcZOh)
  - (Cooler) [Noctua NH-D9 AMP-4926 4U](https://noctua.at/en/nh-d9-amp-4926-4u)
  - (Case Fans) [6x Noctua NF-A12x25 PWM](https://amzn.to/3SUReE7)
  - (Fan Hub) [Noctua NA-FH1 8 channel Fan Hub](https://amzn.to/3SVPL01)

Some of the above links are affiliate links. I have a series of videos showing how I put this system together:

  - Part 1: [How efficient can I build the 100% Arm NAS?](https://www.youtube.com/watch?v=Hz5k5WgTkcc)
  - Part 2: [Silencing the 100% Arm NAS—while making it FASTER?](https://www.youtube.com/watch?v=iD9awxmOGG4)

### Secondary NAS - Raspberry Pi 5 with SATA HAT

<p align="center"><img alt="Raspberry Pi 5 with Jeff Geerling hardware" src="/resources/raspberrypi-5-hardware.jpeg" height="auto" width="600"></p>

The current iteration of the Raspberry Pi 5 SATA NAS I'm running contains the following hardware:

  - (SBC) [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/)
  - (HAT) [Radxa Penta SATA HAT for Pi 5](https://amzn.to/3UyWXBr)
  - (SSDs) [Samsung 870 QVO 8TB SATA SSD](https://amzn.to/3y2nrSR)
  - (microSD) [Kingston Industrial 16GB A1](https://amzn.to/3y2noGF)
  - (Network) [Plugable 2.5GB USB Ethernet Adapter](https://amzn.to/4b9QMt1)
  - (Power) [TMEZON 12V 5A AC adapter](https://amzn.to/3QhYKIw)

Some of the above links are affiliate links. I have a series of videos showing how I put this system together:

  - Part 1: [The ULTIMATE Raspberry Pi 5 NAS](https://www.youtube.com/watch?v=l30sADfDiM8)
  - Part 2: Coming soon.

## Preparing the hardware

The HL15 should not require any special prep, besides having Ubuntu installed. The Raspberry Pi 5 needs its PCIe connection enabled. To do that:

  1. Edit the boot config: `sudo nano /boot/firmware/config.txt`
  2. Add in the following config at the bottom and save the file:

     ```
     dtparam=pciex1
     dtparam=pciex1_gen=3
     ```
  
  3. Reboot

Confirm the SATA drives are recognized with `lsblk`.

## Running the playbook

Ensure you have Ansible installed, and can SSH into the NAS using `ssh user@nas-ip-or-address` without entering a password, then run:

```
ansible-playbook main.yml
```

## Accessing Samba Shares

After the playbook runs, you should be able to access Samba shares, for example the `hddpool/jupiter` share, by connecting to the server at the path:

```
smb://nas01.mmoffice.net/hddpool_jupiter
```

Until [issue #2](https://github.com/geerlingguy/hl15-arm64-nas/issues/2) is resolved, there is one manual step required to add a password for the `jgeerling` user (one time). Log into the server via SSH, run the following command, and enter a password when prompted:

```
sudo smbpasswd -a jgeerling
```

## Benchmarks

There's a disk benchmarking script included, which allows me to test various performance scenarios on the server.

You can run it by copying it to the server, making it executable, and running it with `sudo`:

```
chmod +x disk-benchmark.sh
sudo ./disk-benchmark.sh
```

## License

GPLv3 or later

## Author

Jeff Geerling
