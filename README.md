# Ludus

This project is archived an not actively maintained.

Ludus is a system to build easy to use cyber environments, or "ranges" for testing and development.

Built on [Proxmox](https://www.proxmox.com/en/), Ludus enables advanced automation while still allowing easy manual modifications or setup of virtual machines and networks.

## Docs

All documentation can be found at `https://<ludus IP>:8080/ludus` once your Ludus server is installed.

## Install

!!! Ludus will completely take over the machine! It should not be used for other tasks (i.e. Docker). !!!

Ludus can **only** be installed on a host that meets the following requirements:

- Debian 12
- Supports virtualization - vmx or svm in /proc/cpuinfo (nested virtualization is supported, but has a performance penalty)
- Has at least 32 GB of RAM
- Has at least 200 GB of SSD space (Spinning hard drives not recommended)
- Root access
- Internet access

To install ludus, copy the ludus-server binary to the machine and run it as root. It will copy all files into /opt/ludus and print the configuration values used during install. 

```shell
local:~$ scp ludus-server user@debian12:
local:~$ ssh user@debian12
user@debian12:~$ chmod +x ludus-server
user@debian12:~$ sudo ./ludus-server

Ludus server v0.9.2+e35d94d
No config.yml found. Generating a config at /home/debian/config.yml. Please check that it contains the correct values.
Extracting ludus to /opt/ludus...
Downloading proxmox.py...
Proxmox.py downloaded successfully
Ludus files extracted successfully

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!! Only run Ludus install on a clean Debian 12 machine that will be dedicated to Ludus !!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Using the following config:
---
proxmox_node: ludus
proxmox_interface: ens18
proxmox_local_ip: 10.2.99.240
proxmox_public_ip: 10.2.99.240
proxmox_gateway: 10.2.99.254
proxmox_netmask: 255.255.255.0
proxmox_vm_storage_pool: local
proxmox_vm_storage_format: qcow2
proxmox_iso_storage_pool: local


Ludus install will cause the machine to reboot twice. Install will continue
automatically after each reboot. Check the progress of the install by running:
'ludus-install-status' from a root shell.

Do you want to continue? (y/N):
y
...
```

## Customizing the install

In almost all cases, the default values generated during install are correct. However, if the auto-generated
config contains incorrect values, you can manually create a config called `config.yml` in the same
directory as the ludus-server binary and those values will be used when run.

In advanced setups `/opt/ludus/config.yml` can be modified after install to accommodate different storage pools,
ZFS, etc.


```yaml title="/opt/ludus/config.yml"
---
proxmox_node: ludus               # The proxmox node/hostname for this machine
proxmox_invalid_cert: true        # Disables certificate checking when using the Proxmox API (default true because of the self signed certificates)
proxmox_interface: ens18          # The interface this machine uses to communicate to the internet
proxmox_local_ip: 10.2.99.240     # The IP address for this interface (will be set statically)
proxmox_public_ip: 10.2.99.240    # The public IP address to reach this machine (for use in cloud/NAT environments)
proxmox_gateway: 10.2.99.254      # The gateway this machine uses to reach the internet
proxmox_netmask: 255.255.255.0    # The netmask for the proxmox_interface
proxmox_vm_storage_pool: local    # The name of the VM storage pool - can be changed after install for custom pools
proxmox_vm_storage_format: qcow2  # The VM storage format - can be changed after install (i.e. raw)
proxmox_iso_storage_pool: local   # The storage pool used to store ISOs as they are downloaded for templates - can be changed after install
```

## License

Ludus is licensed under the [CC0](https://creativecommons.org/public-domain/cc0/) license.

## Default Machine Creds
  - Debian based boxes
    - `debian:debian`
  - Redhat based boxes (rocky, alma, centos)
    - `redhat:redhat`
  - Kali
    - `kali:kali` (OS)
    - `kali:password` (KasmVNC)
  - Windows
    - `localuser:password` (local Administrator)
    - `WINDOMAIN\domainuser:password`
    - `WINDOMAIN\domainadmin:password` (Domain Admin)
  - macOS
    - `localuser:password` (local Administrator)
