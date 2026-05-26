# CLI Cheat Sheet 

## 1. Networking & Interface Troubleshooting

### Windows Environment
* **`ipconfig /all`**
    * *Use Case:* Displays full TCP/IP configuration, including MAC addresses (Physical Address) and upstream DNS servers. Crucial for detecting APIPA scope (`169.254.x.x`).
    * *Example:* `ipconfig /all`
* **`ipconfig /release` & `ipconfig /renew`**
    * *Use Case:* Drops the current DHCP lease and requests a fresh IP address configuration from the DHCP pool.
    * *Example:* `ipconfig /renew`
* **`ping`**
    * *Use Case:* Verifies Layer 3 connectivity to a remote host. Stops automatically after 4 packets.
    * *Example:* `ping 10.0.0.1`
* **`tracert`**
    * *Use Case:* Determines the exact network path taken to a destination by listing intermediate routers (hops).
    * *Example:* `tracert 8.8.8.8`

### Linux/Unix Environment
* **`ip addr`**
    * *Use Case:* Displays all network interfaces, assigned IPv4/IPv6 addresses, and the operational status (`UP`/`DOWN`) of the link.
    * *Example:* `ip addr show eth0`
* **`ping -c <count>`**
    * *Use Case:* Tests Layer 3 connectivity. Unlike Windows, Linux pings indefinitely unless restricted by the `-c` flag.
    * *Example:* `ping -c 4 10.0.0.1`
* **`traceroute`**
    * *Use Case:* Linux equivalent for path tracing to identify where packets are being dropped along the route.
    * *Example:* `traceroute 8.8.8.8`

### Cross-Platform Diagnostic Tools
* **`netstat -an`**
    * *Use Case:* Displays all active network connections and listening ports in numerical format. Used to verify if a service (e.g., HTTPS `443`, RDP `3389`) is actually active and binding to an interface.
    * *Example:* `netstat -an`
* **`nslookup`**
    * *Use Case:* Queries DNS servers to test name resolution (FQDN to IP mapping). Works on both OS platforms.
    * *Example:* `nslookup db.enterprise.local`
* **`dig`**
    * *Use Case:* Advanced Linux DNS lookup tool providing detailed DNS record infrastructure data.
    * *Example:* `dig A db.enterprise.local`
* **`arp -a`**
    * *Use Case:* Displays the local ARP cache table matching IP addresses to physical MAC addresses. Essential for isolating IP address conflicts at Layer 2.
    * *Example:* `arp -a`

---

## 2. Storage, Partitioning & File System Management

### Windows Environment
* **`wmic logicaldisk get size,freespace,caption`**
    * *Use Case:* Quick command-line query to check total size and remaining free space on all mounted local volumes.
    * *Example:* `wmic logicaldisk get size,freespace,caption`
* **`diskpart`**
    * *Use Case:* Launches the interactive storage command-line utility used to clean, partition, format, and assign drive letters.
    * *Example:*
        ```cmd
        diskpart
        list disk
        select disk 1
        clean
        create partition primary
        format fs=ntfs quick
        assign letter=E
        exit
        ```
* **`chkdsk /f` & `chkdsk /r`**
    * *Use Case:* Scans the volume for file system integrity errors (`/f`) or physical bad sectors (`/r`) and attempts data recovery.
    * *Example:* `chkdsk D: /f`

### Linux/Unix Environment
* **`df -h`**
    * *Use Case:* Displays total disk space availability across all mounted filesystems in human-readable format (GB/TB).
    * *Example:* `df -h`
* **`du -sh`**
    * *Use Case:* Displays the total aggregated size of a specific directory tree. Excellent for identifying which folder is consuming space.
    * *Example:* `du -sh /var/log`
* **`lsblk`**
    * *Use Case:* Lists information about all available logical block storage devices (HDDs, SSDs, NVMe, and RAID arrays) in a tree structure.
    * *Example:* `lsblk`
* **`fdisk`**
    * *Use Case:* Interactive CLI utility used for manipulating partition tables on Linux storage disks.
    * *Example:* `fdisk /dev/sdb`
* **`fsck`**
    * *Use Case:* File System Consistency Check. Checks and repairs a Linux filesystem. *Warning: The target filesystem must be unmounted first.*
    * *Example:* `umount /dev/sdb1 && fsck -y /dev/sdb1`

---

## 3. System Logs & Incident Inspection

### Windows Environment (Event Viewer Scopes)
Windows logs events into structured binary files (`.evtx`). Use these core categories within the Event Viewer or CLI queries to isolate issues:
* **`System Log`**: Hardware faults, storage controller errors, driver crashes, and power infrastructure events (UPS signals).
* **`Application Log`**: Software application crashes, database operational errors, and service initialization failures.
* **`Security Log`**: Auditing data, including successful/failed login attempts and permission changes.

### Linux/Unix Environment (Plain-Text Analysis)
* **`tail -n <lines>`**
    * *Use Case:* Outputs the final lines of a specific log file to review the most recent error events without loading a massive file into memory.
    * *Example:* `tail -n 20 /var/log/syslog`
* **`tail -f`**
    * *Use Case:* Follows the log output in real-time. Live events scroll down the terminal as they occur. Perfect for dynamic testing.
    * *Example:* `tail -f /var/log/auth.log`
* **`grep`**
    * *Use Case:* Filters text files using matching patterns. Instantly isolates specific occurrences like errors or warnings.
    * *Example:* `grep -i "error" /var/log/syslog`
* **`journalctl -u`**
    * *Use Case:* Queries the `systemd` journal logs specifically filtered for a distinct system daemon or service unit.
    * *Example:* `journalctl -u apache2.service --no-pager`

---
