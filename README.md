# Talos Cluster

Talos Linux based K8s Cluster

## Proxmox

- pve01 - `192.168.100.91/24`
- pve02 - `192.168.100.92/24`
- pve03 - `192.168.100.93/24`
- pve04 - `192.168.100.94/24`
- pve05 - `192.168.100.95/24`

## Controlers

- tlc01 - `192.168.100.101/24`
- tlc02 - `192.168.100.102/24`
- tlc03 - `192.168.100.103/24`

## Workers

- tlw01 - `192.168.100.104/24`
- tlw02 - `192.168.100.105/24`
- tlw03 - `192.168.100.106/24`

## Config PCI passthrough in Proxmox

This portion is optional. I did not have to do it as passthrough worked out of the box for for the Intel 630 graphics card.

Verify IOMMU is enabled in BIOS.  
`dmesg | grep -e DMAR -e IOMMU`

Verify IOMMU interrupy remapping is enabled.  
`dmesg | grep -e 'remapping'`

Verify IOMMU isolation groups.  
`pvesh get /nodes/pve01/hardware/pci --pci-class-blacklist ""`

Blacklist intel GPU kernel module.  
`echo "blacklist i915" >> /etc/modprobe.d/blacklist.conf`

Reboot.  
`reboot`

## Virtual Machines

Create virtual machines.

Control
- 4 x CPU Cores
- 4096MB Memory
- 100GB Disk

Workers
- 2 x CPU Cores
- 2048MB Memory
- 100GB Disk

## Networking

Configure static DHCP address assignments on router.

- tlc01 `BC:24:11:62:13:6E`
- tlc02 `BC:24:11:74:2A:A5`
- tlc03 `BC:24:11:B0:10:1A`

- tlw01 `BC:24:11:43:49:21`
- tlw02 `BC:24:11:D6:2E:0B`
- tlw03 `BC:24:11:A2:E9:C6`

## Talos Linux

Generate a custom image with support for the `qemu-guest-agent` extension.

[ce4c980550dd2ab1b17bbf2b08801c7eb59418eafe8f279833297925d67c7515](https://factory.talos.dev/?arch=amd64&cmdline-set=true&extensions=-&extensions=siderolabs%2Fqemu-guest-agent&platform=metal&target=metal&version=1.9.4)

