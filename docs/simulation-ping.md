# ICMP Simulation Explanation (PC0 → PC2)

##  Scenario

- **Source IP:** 192.168.10.10 (PC0)
- **Destination IP:** 192.168.30.10 (PC2)
- Communication between different VLANs (Inter-VLAN Routing)

---

##  Overview

This simulation demonstrates how an ICMP Echo Request (ping) travels across VLANs using a **Router-on-a-Stick** architecture.

---

##  Step-by-Step Packet Flow

---

##  Packet Creation at PC0

![Simulation Step 1](/images/Simulation-ping-1.png)

PC0 initiates an ICMP Echo Request.

- Source IP: `192.168.10.10`
- Destination IP: `192.168.30.10`

Since the destination is in a different subnet, PC0 sends the frame to its **default gateway**:

- Default Gateway: `192.168.10.1` (Router)

### Key Point:
- The **destination MAC address** is the router’s MAC, not PC2’s
- Layer 3 (IP) remains unchanged
- Layer 2 (MAC) is adjusted for next hop

---

##  Switch Processing (Layer 2)

The switch receives the Ethernet frame.

- It checks the **MAC address table**
- Finds the outgoing port (trunk port Fa0/24)
- The VLAN (10) is allowed on the trunk

### Action:
- The switch **adds an 802.1Q VLAN tag**
- Frame is forwarded to the router

---

##  Router Processing (Layer 3 Routing)

![Simulation Step 2](/images/Simulation-ping-2.png)

The router receives a tagged frame.

### Steps:
1. Removes VLAN tag
2. Decapsulates Ethernet frame
3. Processes IP packet

### Routing decision:
- Destination: `192.168.30.10`
- Matches network: `192.168.30.0/24`

### Action:
- Router prepares new frame:
  - New **source MAC** → router interface (VLAN 30)
  - New **destination MAC** → PC2

### Important:
- TTL decreases (128 → 127)
- IP remains unchanged

---

##  Switch Forwarding to PC2

The switch receives the new frame.

- Checks MAC table
- Finds correct port for PC2 (access port VLAN 30)

### Action:
- Removes VLAN tag (access port)
- Sends frame to PC2

---

##  Packet Reception at PC2

![Simulation Step 3](/images/Simulation-ping-3.png)

PC2 receives the frame.

### Validation:
- MAC address matches → accepts frame
- IP address matches → processes packet

### Result:
- ICMP Echo Request received

---

##  ICMP Reply (Return Path)

PC2 generates an ICMP Echo Reply:

- Source IP: `192.168.30.10`
- Destination IP: `192.168.10.10`

### Important:
- Sends reply to **default gateway**
- Same process occurs in reverse:
  - PC2 → Switch → Router → Switch → PC0

---

##  Summary of Key Concepts

### Layer 2 (Switch)
- Works with MAC addresses
- Adds/removes VLAN tags
- Forwards frames based on MAC table

### Layer 3 (Router)
- Routes between VLANs
- Uses IP routing table
- Rewrites Layer 2 headers

### VLAN Behavior
- VLANs isolate traffic at Layer 2
- Routing is required for inter-VLAN communication

---

##  Key Takeaways

- Devices in different VLANs cannot communicate without a router
- Router-on-a-Stick uses subinterfaces + 802.1Q tagging
- Each hop rewrites Layer 2, but not Layer 3
- Switches do not route, only forward frames

---

##  Related Files

- `/images/Simulation-ping-1.png`
- `/images/Simulation-ping-2.png`
- `/images/Simulation-ping-3.png`
