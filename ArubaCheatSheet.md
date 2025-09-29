# 📘 Aruba Switch – VLAN, MAC, ARP & Port Mapping Cheat Sheet

## **1. Basic Switch Info**

```bash
show version
```

➡️ Shows model, software version, serial number, uptime

```bash
show system
```

➡️ Shows hostname, contact, location

```bash
show running-config
```

➡️ Displays full running configuration

---

## **2. VLAN Information**

```bash
show vlan
```

➡️ Lists VLANs, their names, and member ports

```bash
show vlan 2002
```

➡️ Shows ports assigned to VLAN 2002

---

## **3. MAC Address Table**

```bash
show mac-address
```

➡️ Shows all MAC → Port mappings

```bash
show mac-address vlan 2002
```

➡️ Filters MACs belonging to VLAN 2002

```bash
show mac-address port 1/16
```

➡️ Shows devices on a specific port

---

## **4. ARP Table**

```bash
show arp
```

➡️ Lists all IP → MAC mappings

```bash
show arp vlan 2002
```

➡️ Shows ARP entries for VLAN 2002

---

## **5. Tracing IP → Port**

Steps:

1. Ping the IP from switch:

   ```bash
   ping 10.50.2.3
   ```

   (If alive, it will be in the ARP table)

2. Find its MAC:

   ```bash
   show arp vlan 2002
   ```

   (Look for `10.50.2.3 → MAC`)

3. Find the port:

   ```bash
   show mac-address vlan 2002 | include <MAC>
   ```

➡️ This gives you **IP → MAC → Port mapping**

---

## **6. Neighbors**

```bash
show lldp info remote
```

➡️ Shows directly connected neighbors (switches/routers/phones)

```bash
show lldp info local
```

➡️ Shows your switch’s advertised info

---

## **7. Spanning Tree**

```bash
show spanning-tree
```

➡️ Root bridge and STP state per VLAN

```bash
show spanning-tree vlan 2002
```

➡️ STP details only for VLAN 2002

---

## **8. Interfaces & Counters**

```bash
show interfaces brief
```

➡️ Quick view: port status, VLAN, speed, duplex

```bash
show interfaces counters
```

➡️ Error counters (drops, collisions, etc.)

```bash
show interfaces 1/16
```

➡️ Detailed stats for a specific port

---

## **9. Logs & Monitoring**

```bash
show log -r
```

➡️ Recent events (link flaps, errors)

```bash
show process cpu
show memory
```

➡️ Resource utilization

---

## **10. Saving Config**

```bash
write memory
```

➡️ Save current running-config to startup-config

---

✅ **Summary Workflow for Device Tracing in VLAN 2002**

1. `ping <IP>` → verify alive
2. `show arp vlan 2002` → get MAC
3. `show mac-address vlan 2002` → find port
4. `show interfaces <port>` → get port details
5. (Optional) `show lldp info remote` → identify connected device

