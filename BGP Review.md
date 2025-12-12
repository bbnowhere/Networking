# 🔔 **1. Understanding the BGP Notification You Received**

### **BGP Neighbors Summary**

| Label    | Neighbor IP   | AS    | State       | Uptime | Status      |
| -------- | ------------- | ----- | ----------- | ------ | ----------- |
| **NKN**  | IP ADDRESS | 55824 | Established | 1w5d   | Established |
| **TTSL** | IP ADDRESS | 45820 | Established | 2w0d   | Established |

### ✔ Meaning:

* Both BGP neighbors are **UP**.
* State **Established** = routes are being exchanged successfully.
* Uptime shows stable, no recent flapping.
* No neighbors are down.

### **BGP Nexthops**

Both next-hop IPs are **REACHABLE**, meaning forwarding will work without issues.

---

# 🧠 **2. BGP States You Learned**

Only **ESTABLISHED** is healthy.

| State           | What It Means                                           |
| --------------- | ------------------------------------------------------- |
| **Idle**        | Not trying to connect (problem).                        |
| **Active**      | Trying repeatedly but failing — usually neighbor issue. |
| **Connect**     | TCP handshake in progress.                              |
| **OpenSent**    | Waiting for OPEN msg reply.                             |
| **OpenConfirm** | Waiting for KEEPALIVE.                                  |
| **Established** | SUCCESS — routes exchanged.                             |

---

# 🔍 **3. How to Check BGP Status (Cisco Commands)**

### Check all neighbors:

```
show ip bgp summary
```

### Check detailed session:

```
show ip bgp neighbors <IP>
```

### Check BGP routes:

```
show ip bgp
show ip route bgp
```

### Check next-hop reachability:

```
show ip cef <nexthop-ip>
```

---

# **4. How to Troubleshoot When BGP Goes Down**

### Step-by-step:

1. **Check Interface**

```
show interfaces <interface>
```

2. **Ping Neighbor**

```
ping <neighbor-ip>
traceroute <neighbor-ip>
```

3. **Check TCP Port 179**
   (Firewall or ACL may block BGP)

4. **Verify BGP Config**

```
show run | section bgp
```

5. **Check Logs**

```
show log
```

6. **Check Authentication (MD5)**
   Password mismatch = BGP reset.

7. **Check Route Limits**
   Neighbor may send too many prefixes.

---

# 📡 **5. How to See Prefixes Received**

### View quick summary:

```
show ip bgp summary
```

### View all received routes:

```
show ip bgp neighbors <IP> received-routes
```

### View best routes used:

```
show ip bgp neighbors <IP> routes
```

---

# 🧭 **6. BGP Next-Hop Reachability**

A BGP route is installed **only if next-hop is reachable**.

Check with:

```
show ip cef <nexthop-ip>
```

Unreachable next-hop = route not used.

---

# 🔐 **7. How to Prevent Route Leaks**

### Use prefix-lists (best practice):

```
neighbor <IP> prefix-list FILTER-IN in
neighbor <IP> prefix-list FILTER-OUT out
```

### Use AS-PATH filtering:

```
neighbor <IP> filter-list <num> out
```

### Use max-prefix protection:

```
neighbor <IP> maximum-prefix <limit> <warn-percent>
```

This avoids crashes or large leaks.

---

# 🔄 **8. How to Reduce BGP Flapping**

* Enable **BGP dampening**

```
bgp dampening
```

* Increase BGP timers
* Ensure stable next-hop
* Check router CPU / memory

---

# 🧾 **Summary of Everything You Learned Today**

You learned:

✔ How to read a BGP notification and confirm neighbor health
✔ Meaning of BGP states (Idle, Active, Established…)
✔ How to check BGP status using Cisco commands
✔ How to troubleshoot BGP when it goes down
✔ How to view prefixes received from a neighbor
✔ Importance of next-hop reachability
✔ How to prevent BGP route leaks
✔ How to reduce BGP flapping for stability
✔ That your current BGP status is **healthy** with both neighbors established and reachable

---
