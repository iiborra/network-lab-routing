# Network Lab - Inter - VLAN Routing (Router-on-a-Stick)

## Objective
The goal of this lab is to enable communication between different VLANs using a Router-on-a-Stick configuration

## What is demonstrated
- Basic routing concepts
- Inter-VLAN communication
- Network segmentation and design
- Cisco IOS configuration skills

## Topology
This lab includes:
- 1 Router
- 1 Layer 2 Switch
- Multiple PCs in different VLANs

VLANs used:
- VLAN 10 → Administration
- VLAN 20 → Guests
- VLAN 30 → IoT


(See topology diagram in /topology)

## Technologies
- Cisco IOS
- VLAN (802.1Q)
- Static routing
- Subinterfaces
- Router-on-a-Stick

## Configuration

### Switch Configuration
- VLAN creation
- Access ports assignment
- Trunk port configuration

### Router Configuration
- Subinterfaces
- Encapsulation dot1Q
- IP addressing

### End Devices Configuration

| Device | Interface | IP Address      | Default Gateway | VLAN |
|--------|----------|----------------|----------------|------|
| PC0    | Fa0      | 192.168.10.10  | 192.168.10.1   | 10   |
| PC1    | Fa0      | 192.168.20.10  | 192.168.20.1   | 20   |
| PC2    | Fa0      | 192.168.30.10  | 192.168.30.1   | 30   |

### VLAN Configuration

| VLAN ID | Name           | Network           | Gateway         |
|--------|---------------|------------------|----------------|
| 10     | ADMIN         | 192.168.10.0/24  | 192.168.10.1   |
| 20     | GUEST         | 192.168.20.0/24  | 192.168.20.1   |
| 30     | IOT           | 192.168.30.0/24  | 192.168.30.1   |

### Interface Assignment

| Device   | Interface       | Connected To        | Mode    | VLAN |
|----------|----------------|---------------------|--------|------|
| Switch0  | Fa0/1          | PC0                 | Access | 10   |
| Switch0  | Fa0/2          | PC1                 | Access | 20   |
| Switch0  | Fa0/3          | PC2                 | Access | 30   |
| Switch0  | Fa0/24         | Router G0/0         | Trunk  | All  |
| Router   | G0/0.10        | Switch0 (VLAN 10)   | L3     | 10   |
| Router   | G0/0.20        | Switch0 (VLAN 20)   | L3     | 20   |
| Router   | G0/0.30        | Switch0 (VLAN 30)   | L3     | 30   |

### Router Subinterfaces

| Interface        | VLAN | IP Address       | Encapsulation |
|-----------------|------|------------------|--------------|
| G0/0.10         | 10   | 192.168.10.1     | dot1Q 10     |
| G0/0.20         | 20   | 192.168.20.1     | dot1Q 20     |
| G0/0.30         | 30   | 192.168.30.1     | dot1Q 30     |

## Testing
- Ping between devices in different VLANs
- Verify routing functionality
- Check trunk operation

## Screenshots
Available in /screenshots:
- VLAN configuration
- Routing tests
- Packet flow simulation

## Files
- Router configuration → /configs/router.txt
- Switch configuration → /configs/switch.txt
- Packet Tracer lab → /packet-tracer/lab.pkt

## Key Concepts
- Inter-VLAN routing
- Trunking (802.1Q)
- Subinterfaces
- Default gateway per VLAN

## Explanation
In a Layer 2 network, VLANs isolate traffic. To allow communication between VLANs, a Layer 3 device is required.

This lab uses a single router interface configured with multiple subinterfaces, each associated with a VLAN using 802.1Q encapsulation.

The switch sends tagged traffic to the router, which routes packets between VLANs and sends them back to the switch.

This design is known as **Router-on-a-Stick**.