# CCNA-Day-13-Floating-Static-Routes-Backup-Routes-

## Overview

In Day 12, you learned how to configure a Default Route. In this lab, you will learn how to configure a **Floating Static Route**, which provides a backup path if the primary route fails.

A Floating Static Route has a higher **Administrative Distance (AD)** than the primary static route, so it remains inactive until the primary route becomes unavailable.

This concept is commonly used in enterprise networks to provide redundancy and improve network availability.

---

# Lab Objectives

After completing this lab, you will be able to:

- Understand Administrative Distance (AD)
- Configure Primary Static Routes
- Configure Floating Static Routes
- Verify route failover
- Test redundant routing
- Troubleshoot routing issues

---

# Note

The Cisco **1941 Router** available in Packet Tracer has only two Gigabit Ethernet interfaces (G0/0 and G0/1). Because one interface is used for the LAN and the other for the router-to-router connection, a true redundant physical topology cannot be built with this router.

For this reason, this lab focuses on learning the configuration and concept of Floating Static Routes. In future enterprise labs, routers with additional interfaces will be used to demonstrate real failover.

---

# Logical Topology

```text
          Primary Route
LAN1 ---- R1 ================= R2 ---- LAN2
            \
             \
              Backup Route (Higher AD)
```

---

# IP Addressing Plan

## LAN 1

| Device | Interface | IP Address |
|---------|-----------|------------|
| PC1 | Fa0 | 192.168.1.10/24 |
| R1 | G0/0 | 192.168.1.1/24 |

---

## Router Link

| Device | Interface | IP Address |
|---------|-----------|------------|
| R1 | G0/1 | 10.10.10.1/30 |
| R2 | G0/1 | 10.10.10.2/30 |

---

## LAN 2

| Device | Interface | IP Address |
|---------|-----------|------------|
| R2 | G0/0 | 192.168.2.1/24 |
| PC2 | Fa0 | 192.168.2.10/24 |

---

# Primary Static Route

On **R1**

```bash
ip route 192.168.2.0 255.255.255.0 10.10.10.2
```

On **R2**

```bash
ip route 192.168.1.0 255.255.255.0 10.10.10.1
```

The Administrative Distance of a normal static route is **1**.

---

# Floating Static Route

A Floating Static Route is configured with a higher Administrative Distance.

Example:

On **R1**

```bash
ip route 192.168.2.0 255.255.255.0 10.10.10.2 10
```

On **R2**

```bash
ip route 192.168.1.0 255.255.255.0 10.10.10.1 10
```

Here:

- Next-hop IP remains the same.
- Administrative Distance is **10**.
- The route stays inactive until the primary route fails.

---

# Understanding Administrative Distance

| Route Type | Administrative Distance |
|------------|-------------------------|
| Connected | 0 |
| Static Route | 1 |
| Floating Static Route | 10 (or any value greater than 1) |

A router always chooses the route with the **lowest Administrative Distance**.

---

# Verification Commands

View the routing table:

```bash
show ip route
```

View interface status:

```bash
show ip interface brief
```

View configured routes:

```bash
show running-config
```

Test connectivity:

```bash
ping 192.168.2.10
```

---

# Expected Behavior

- The router installs the primary static route in the routing table.
- The floating static route is kept as a backup.
- If the primary route becomes unavailable, the backup route automatically becomes active.

---

# Troubleshooting Scenarios

## Scenario 1

Configure both routes with the same Administrative Distance.

Result:

- Floating route will not function as a backup.

---

## Scenario 2

Configure the wrong next-hop address.

Result:

- Route cannot be used.

---

## Scenario 3

Forget to configure the return route.

Result:

- Ping request reaches the destination.
- Reply cannot return.

---

## Scenario 4

Configure an Administrative Distance lower than the primary route.

Result:

- The backup route becomes the preferred route.

---

# Mini Challenge

Answer the following questions:

1. What is the default Administrative Distance of a static route?
2. Why must a Floating Static Route have a higher Administrative Distance?
3. Which route is selected when two static routes exist?
4. What happens if the primary route fails?
5. Which command displays the routing table?

---

# Skills Learned

- Administrative Distance
- Primary Static Route
- Floating Static Route
- Route Selection
- Routing Table Analysis
- Route Failover Concept
- Routing Troubleshooting

---

# Conclusion

This lab introduced the concept of Floating Static Routes and Administrative Distance. Although the standard Packet Tracer 1941 router cannot demonstrate redundant physical links, understanding this concept is essential because it is widely used in enterprise networks to provide backup connectivity and improve network reliability.

---

# Author

**Muhammad Kausar**

CCNA Enterprise Networking Lab Series
