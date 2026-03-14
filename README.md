# Project HADES – Distributed Relay Control (ESP8266 + ESP-NOW)

## Overview

This project is an experimental stage of **Project HADES**, designed to explore the creation of a **distributed automation network using ESP8266 devices**.

The system implements a **controller-node architecture**, where one ESP8266 device acts as a central controller providing a web interface and managing communication with multiple ESP-01 relay nodes through **ESP-NOW**.

Unlike traditional IoT systems that rely on routers or cloud services, this architecture uses **direct peer-to-peer wireless communication**, enabling low latency, reduced network complexity, and autonomous device discovery.

The main goal of this version was to experiment with:

- distributed embedded systems
- device discovery and registration
- peer-to-peer wireless communication
- remote relay control
- embedded web interfaces
- device lifecycle monitoring

This version represents a significant step between the **initial Twin Relays prototype** and the more complex **HADES interlock system architecture**.

---

## System Architecture

The system is composed of a **central controller** and one or more **relay nodes**.

| Device                 | Role                                          |
| ---------------------- | --------------------------------------------- |
| **ESP8266 Controller** | Hosts the web interface and manages devices   |
| **ESP01 Relay Node**   | Controls relay hardware and executes commands |

The controller creates a Wi-Fi network and manages all connected nodes.

```
User (Phone / PC)
        │
        │ WiFi
        ▼
ESP8266 Controller
 Web Interface
 Device Manager
        │
        │ ESP-NOW
        ▼
ESP01 Relay Nodes
        │
        ▼
      Relays
```

Each relay node operates as an independent device capable of receiving commands, reporting status and maintaining its internal state.

---

## Communication

Communication between devices uses **ESP-NOW**, a wireless protocol that allows ESP8266 devices to exchange data directly without requiring a router.

Advantages of ESP-NOW include:

- very low communication latency
- minimal protocol overhead
- direct device-to-device communication
- reliable local networks
- reduced infrastructure requirements

The controller periodically performs **device discovery** to detect nodes and maintain an updated list of active devices.

### Message Types

The firmware implements a lightweight messaging structure used for communication.

| Message              | Purpose                         |
| -------------------- | ------------------------------- |
| `DISCOVERY`          | Controller searches for devices |
| `DISCOVERY_RESPONSE` | Node identifies itself          |
| `RELAY_COMMAND`      | Activate or deactivate relay    |
| `HEARTBEAT`          | Node reports it is still active |
| `STATUS`             | Node reports relay state        |

This mechanism allows the controller to automatically detect and monitor devices on the network.

---

## Web Interface

The controller hosts an **embedded web server** that provides a simple control interface.

The ESP8266 operates as a **Wi-Fi Access Point**, allowing users to connect directly using a smartphone or computer.

Example network configuration:

```
SSID: ESP-Relay-Control
Password: 12345678
```

Once connected, the web interface can be accessed at:

```
http://192.168.4.1
```

The interface allows users to:

- view connected devices
- trigger relay nodes
- monitor device status
- control multiple relays

This web interface provides a lightweight control panel without requiring external applications or cloud services.

---

## Relay Node Behavior

Each ESP01 node functions as a **smart relay device**.

When a command is received from the controller:

1. The node processes the received command.
2. The relay is activated.
3. A timer may be started depending on the command.
4. The relay is automatically deactivated after the delay.
5. The node reports its status back to the controller.

Example execution flow:

```
Command received
      ↓
Relay ON
      ↓
Timer started
      ↓
Delay elapsed
      ↓
Relay OFF
```

Nodes also periodically send **heartbeat messages** so the controller can detect if a device goes offline.

---

## Device Lifecycle Management

The controller maintains a list of known devices and tracks their activity.

The system performs:

- automatic device discovery
- device registration
- heartbeat monitoring
- inactive device cleanup

This ensures the network remains synchronized and that offline devices are detected automatically.

---

## Hardware

### Controller

- ESP8266 development board (NodeMCU or similar)

### Relay Nodes

- ESP-01 (ESP8266)
- relay module

### GPIO Example

| Pin   | Function      |
| ----- | ------------- |
| GPIO0 | Relay control |
| GPIO2 | Status LED    |

---

## Potential Applications

Although initially designed as a prototype, this architecture can be used for several applications:

- remote relay control
- electric gate triggers
- access control systems
- automation prototypes
- distributed device networks

---

## Future Development

The architecture allows the system to evolve into a more advanced distributed automation platform.

Possible future improvements include:

- support for multiple relay nodes
- sensor nodes (door, motion, temperature)
- automation rules
- event logging
- improved web dashboard
- security and authentication

---

## Relationship to Project HADES

This project represents an intermediate stage in the development of **Project HADES**.

It introduced key architectural concepts such as:

- controller-node architecture
- device discovery
- distributed device management
- embedded web interfaces
- wireless automation networks

These concepts later influenced the design of the **HADES distributed interlock system**.

---

## Author

Developed by **Davi Han Ko**
