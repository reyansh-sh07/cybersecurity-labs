# Linux Network Interface and Socket Inspection

**Section:** Foundations  
**Focus:** Network interfaces, IP addressing, routing tables, and listening sockets

## Objective

Understand how to inspect the network configuration of a Linux system, identify its active network interfaces and IP addresses, view the routing table, and examine TCP/UDP sockets.

## Environment

- **Operating system:** Ubuntu running under WSL2
- **Shell:** Bash

## Commands Practised

| Command | What it does | Why it is useful |
|---|---|---|
| `ip -brief addr` | Displays a concise summary of network interfaces, their states, and assigned IP addresses. | Quickly identifies active interfaces and their addressing configuration. |
| `ip route` | Displays the kernel's IPv4 routing table. | Helps identify the default gateway and routes used to reach networks. |
| `ss -tuln` | Displays listening TCP and UDP sockets using numeric addresses and ports. | Helps identify services that are listening for network traffic. |

## Core Concepts

### 1. Network Interfaces and IP Addresses

A **network interface** is a system's connection point to a network. It can be physical or virtual.

- **`lo` (loopback):** A virtual interface used by a system to communicate with itself. The common IPv4 loopback address is `127.0.0.1`.
- **`eth0`:** A network interface commonly presented to Linux environments, including virtualized and WSL2 environments.
- **Interface state:** `UP` indicates that the interface is administratively enabled. It does not, by itself, prove that external connectivity is working.
- **IP address and prefix:** An address such as `192.0.2.10/24` combines an IP address with a prefix length that identifies the network portion.

### 2. Routing Table and Default Gateway

The routing table determines where the system sends network packets.

- **Default route (`default`):** Used when no more specific route matches the destination.
- **Gateway:** The next-hop router used by the default route.
- **Connected route:** A route for a network directly reachable through a local interface.
- **`dev`:** Identifies the interface used for a route.
- **`src`:** Indicates the preferred source address for traffic using that route.

The default gateway is not necessarily the destination server; it is the next hop used to forward traffic beyond the directly connected network.

### 3. Listening Network Sockets

A **socket** is an endpoint used by a process for network communication. The `ss` utility displays socket information.

In `ss -tuln`:

- **`-t`:** Show TCP sockets.
- **`-u`:** Show UDP sockets.
- **`-l`:** Show listening sockets.
- **`-n`:** Display numeric addresses and port numbers instead of resolving names.

Important states in the output:

- **LISTEN (TCP):** A TCP socket is waiting for incoming connection requests.
- **UNCONN (UDP):** A UDP socket is unconnected. This is normal because UDP does not establish TCP-style connections.

A listening socket indicates that a service is accepting or awaiting traffic on an address and port. It does not automatically mean that the service is exposed to the public internet; reachability also depends on its bind address, routing, firewall rules, and network environment.

## Key Takeaways

- `ip -brief addr` provides a quick overview of interfaces and assigned addresses.
- `ip route` helps explain how packets leave the local network and which gateway is used.
- `ss -tuln` helps identify listening TCP and UDP sockets.
- Loopback addresses are for local communication; private interface addresses are used within private or virtual networks.
- Interface state, a routing entry, or a listening socket alone does not prove end-to-end network connectivity or external exposure.

## Evidence and Privacy

No terminal screenshot is published for this exercise. Network configuration output can reveal interface addresses, gateways, and listening services. The exercise is therefore documented through concepts and command explanations only.

## Scope

This was a foundational network-inspection exercise performed in my own Ubuntu/WSL2 environment. It involved inspecting local network configuration and sockets, not scanning or testing external systems.
