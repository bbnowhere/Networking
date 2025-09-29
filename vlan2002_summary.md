## **1. MAC Address Table (Layer 2)**

### What you observed:

* Most MAC addresses are on **port 1/A1**, indicating:

  * A **virtualization host** (many VMs)
  * Or another **switch/hub** connecting multiple devices
* Other MAC addresses appear on ports like `1/1`, `1/3`, `1/5` … `1/24`, representing individual devices.
* MAC addresses are grouped by **vendor prefix**, helping identify device types.

### Key command:

```bash
show mac-address vlan 2002
```

* **Output:** MAC → Port mapping

---

## **2. ARP Table (Layer 3)**

### What you observed:

* ARP shows **IP ↔ MAC mappings** for devices actively sending traffic.
* Initially empty; after pinging `10.50.2.1`, the switch learned it.

### Key command:

```bash
show arp vlan 2002
```

* **Output:** IP → MAC → Port
* Only shows **active IP devices**.

---

## **3. Mapping IP to Port**

### Process:

1. **Ping the device** (so it generates ARP traffic):

```bash
ping 10.50.2.x
```

2. **Check the ARP table** to get its MAC:

```bash
show arp vlan 2002
```

3. **Match the MAC in the MAC table** to find the port:

```bash
show mac-address vlan 2002 | include <MAC>
```

**Why:**

* Ping alone doesn’t tell the port.
* Switch learns MACs at Layer 2, IPs at Layer 3 (ARP).

---

## **4. Scanning the entire subnet**

* Manual ping commands:

```
ping 10.50.2.1
ping 10.50.2.2
...
ping 10.50.2.255
```

* Or automated:

**Linux:**

```bash
for i in {1..255}; do ping -c 1 10.50.2.$i; done
```

**Windows PowerShell:**

```powershell
1..255 | ForEach-Object { ping 10.50.2.$_ -n 1 }
```

**Purpose:**

* Generate ARP entries for all active hosts.
* Then check `show arp vlan 2002` and `show mac-address vlan 2002` to map **IP → MAC → Port**.

---

## **5. Identifying device types**

* MAC prefixes (OUI) hint at vendor or type:

  * `0002d1` → VMware/Cisco virtual interfaces
  * `3817c3` → HP/Aruba or other vendor
  * `d46137` → another vendor/device cluster
* Helps distinguish **virtual hosts** from **physical devices**.

---

## **6. Optional: Network discovery commands**

* **Check neighbors (optional, useful for mapping)**:

```bash
show cdp neighbors detail    # Cisco
show lldp neighbors detail   # LLDP-capable switches
```

* Can give **device IP, MAC, port, and name** without pinging all hosts.

---

###  **Summary**

1. **MAC table** → shows all devices reachable on each switch port (Layer 2).
2. **ARP table** → shows active IP ↔ MAC mappings (Layer 3).
3. **IP to port mapping** → ping IP → check ARP → match MAC in MAC table.
4. **Automated ping** → quickly generates ARP entries for the entire VLAN.
5. **OUI analysis** → identify virtual vs. physical devices.
6. **Neighbor discovery** → optional for easier network mapping.

---
