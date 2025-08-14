# Complete Network Device Inventory

## Network Map Summary
- **Total Devices**: 46 devices
- **Static IP Range**: 192.168.1.1 - 192.168.1.106
- **DHCP Range**: 192.168.1.107 - 192.168.1.254
- **Router/Gateway**: OPNsense at 192.168.1.1

## Static IP Assignments (192.168.1.1 - 192.168.1.106)

### Network Infrastructure (192.168.1.1-9)
| IP | Device | MAC Address | Description |
|----|--------|-------------|-------------|
| 192.168.1.1 | pve-opnsense-vm-router | 5e:17:e3:93:1c:c6 | OPNsense Router VM |
| 192.168.1.2 | tplink-sg3428-2-switch | 5c:a6:e6:be:9f:93 | TP-Link Managed Switch |
| 192.168.1.5 | linksys-mx4200-joe-router | c4:41:1e:fa:dd:d4 | Linksys Mesh Router (Joe) |
| 192.168.1.6 | linksys-mx4200-kitchen-router | c4:41:1e:fa:dd:cf | Linksys Mesh Router (Kitchen) |
| 192.168.1.7 | linksys-mx4200-family-router | c4:41:1e:fa:dd:bb | Linksys Mesh Router (Family) |
| 192.168.1.8 | linksys-mx4200-mark-router | e8:9f:80:47:29:4f | Linksys Mesh Router (Mark) |
| 192.168.1.9 | linksys-mx4200-gazebo-router | e8:9f:80:47:2a:85 | Linksys Mesh Router (Gazebo) |

### Storage & Security (192.168.1.10-29)
| IP | Device | MAC Address | Description |
|----|--------|-------------|-------------|
| 192.168.1.10 | synology-1520-nas | 00:11:32:ff:4a:a5 | Synology DS1520+ NAS |
| 192.168.1.20 | reolink-nvr | ec:71:db:07:42:84 | Reolink NVR Security System |
| 192.168.1.21 | epson-et-3830-printer | e0:bb:9e:2f:da:a2 | Epson ET-3830 Printer |

### Virtualization Infrastructure (192.168.1.40-60)
| IP | Device | MAC Address | Description |
|----|--------|-------------|-------------|
| 192.168.1.40 | pve-proxmox-host | 7c:2b:e1:13:92:4b | Proxmox VE Host |
| 192.168.1.50 | pve-wireguard-lxc | bc:24:11:80:09:6a | WireGuard VPN Container |
| 192.168.1.51 | pve-tailscale-lxc | bc:24:11:3d:4e:33 | Tailscale VPN Container |
| 192.168.1.52 | pve-omada-lxc | bc:24:11:6b:8c:41 | Omada Controller Container |
| 192.168.1.53 | pve-netbox-lxc | bc:24:11:84:d2:72 | NetBox IPAM Container |
| 192.168.1.54 | pve-iventoy-lxc | bc:24:11:f5:2a:99 | iVentoy PXE Boot Container |
| 192.168.1.55 | pve-docker-lxc | bc:24:11:57:55:cd | Docker Services Container |
| 192.168.1.56 | pve-syncthing-lxc | bc:24:11:d3:bb:a8 | Syncthing Container (Note: Listed as umbrel-vm) |
| 192.168.1.60 | pve-umbrel-vm | bc:24:11:d3:bb:a8 | Umbrel VM |

### Workstations & Laptops (192.168.1.100-106)
| IP | Device | MAC Address | Description |
|----|--------|-------------|-------------|
| 192.168.1.100 | dell-inspiron-3847 | f8:bc:12:95:dd:10 | Dell Inspiron 3847 Desktop |
| 192.168.1.101 | dell-inspiron-3030 | c4:5a:b1:fa:d0:68 | Dell Inspiron 3030 Desktop |
| 192.168.1.102 | pc-windows-7-vm | 08:00:27:4a:0e:1c | Windows 7 VM |
| 192.168.1.103 | hp-dv9500-pavilion-eth | 00:1b:24:87:0c:99 | HP Pavilion (Ethernet) |
| 192.168.1.104 | hp-dv9500-pavilion-wifi | 84:14:4d:d8:c0:1b | HP Pavilion (WiFi) |
| 192.168.1.105 | msi-ge75-raider-9sf-eth | 00:d8:61:08:48:3d | MSI Raider (Ethernet) |
| 192.168.1.106 | msi-ge75-raider-9sf-wifi | 5c:b4:7e:25:c7:bd | MSI Raider (WiFi) |

## DHCP Devices (192.168.1.107+)

### Smart Home Devices
| Current IP | Device | MAC Address | Description |
|------------|--------|-------------|-------------|
| 192.168.1.107 | google-nest-thermostat | 18:b4:30:08:2c:7b | Nest Thermostat |
| 192.168.1.222 | irobot-roomba-vacuum | 50:14:79:4c:b8:16 | Roomba Vacuum |

### Apple Devices
| Current IP | Device | MAC Address | Description |
|------------|--------|-------------|-------------|
| 192.168.1.178 | apple-ipad | 3e:46:0c:c2:ce:db | iPad |
| 192.168.1.188 | apple-imac | ca:8b:93:36:cc:c9 | iMac |
| 192.168.1.218 | apple-iphone-16-pro-max | da:8a:05:f2:dd:c0 | iPhone 16 Pro Max |
| 192.168.1.136 | apple-iphone | ca:74:ef:d8:66:69 | iPhone |
| 192.168.1.210 | apple-watch | 42:35:02:5c:69:df | Apple Watch |
| - | apple-ipad | de:4b:1d:cb:68:8b | iPad (offline) |
| - | apple-imac | 96:2b:f3:dd:b5:cd | iMac (offline) |
| - | apple-macbook-air | e6:3a:13:34:2f:51 | MacBook Air (offline) |
| - | apple-watch-se | ce:46:f9:2f:19:13 | Apple Watch SE (offline) |
| - | apple-iphone | 36:37:89:6b:a7:6a | iPhone (offline) |
| - | apple-iphone | 46:98:a7:1e:28:c6 | iPhone (offline) |
| - | apple-iphone | 06:01:82:7e:11:3e | iPhone (offline) |

### Samsung Devices
| Current IP | Device | MAC Address | Description |
|------------|--------|-------------|-------------|
| 192.168.1.111 | samsung-galaxy-s22-ultra | 9a:c2:92:bc:08:e6 | Galaxy S22 Ultra |
| 192.168.1.129 | samsung-galaxy-a03s | 7e:ce:ea:ff:bd:0b | Galaxy A03s |
| 192.168.1.144 | samsung-galaxy-j7-v | 10:8e:e0:ad:98:22 | Galaxy J7 V |

### Other Devices
| Current IP | Device | MAC Address | Description |
|------------|--------|-------------|-------------|
| 192.168.1.197 | alienware-18-area51 | 64:4b:f0:60:09:40 | Alienware 18 (This machine) |
| 192.168.1.208 | amazon | ec:0d:e4:34:87:37 | Amazon Device |
| 192.168.1.217 | amazon-fire-tv-4k | ec:8a:c4:84:e8:61 | Fire TV 4K |
| - | zte-libero-5g | f6:38:02:28:7c:ff | ZTE Libero 5G II (offline) |
| - | vizio-tv | 00:6b:9e:67:30:8b | Vizio TV (offline) |
| - | lg-tv | 38:8c:50:47:a1:e8 | LG TV (offline) |
| - | lenovo-thinkpad | 00:c2:c6:20:15:ed | Lenovo ThinkPad (offline) |
| - | lenovo-thinkpad | 28:d2:44:42:cf:67 | Lenovo ThinkPad (offline) |

### Unknown Devices
| Current IP | Device | MAC Address | Description |
|------------|--------|-------------|-------------|
| - | unknown | 9a:a8:39:c4:ba:5c | Unknown device (offline) |
| - | unknown | 22:e3:b8:3c:80:ca | Unknown device (offline) |

## Network Statistics
- **Static IP Devices**: 29 devices
- **Currently Active DHCP**: 17 devices
- **Total Configured**: 46 devices
- **SSH Accessible**: 14 devices (OPNsense, Proxmox, LXCs, NAS, Laptops)

## Notes
- All devices 192.168.1.1-106 have static DHCP reservations in OPNsense
- DHCP pool starts at 192.168.1.107
- Linksys MX4200 routers form a mesh network (5 nodes)
- Multiple network interfaces on some devices (HP and MSI laptops have both ethernet and WiFi IPs)