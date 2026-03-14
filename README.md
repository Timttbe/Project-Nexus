# Project NEXUS – Distributed IoT Control Platform

## Overview

Project NEXUS is an experimental distributed IoT control platform built using ESP8266 microcontrollers. The goal of the project is to explore the development of a decentralized device network capable of communication, discovery, synchronization and remote control over a local Wi-Fi infrastructure.

Instead of relying on cloud services or centralized servers, NEXUS focuses on a lightweight peer-to-peer architecture where each device participates in the network, shares its status, and reacts to commands in real time.

The project was designed as a foundation for building more advanced automation systems, similar in concept to platforms such as Home Assistant, but implemented at a low-level embedded systems layer. The objective is to create a modular environment where devices such as relays, sensors and controllers can automatically discover each other and cooperate as part of a distributed system.

This project originated as an evolution of earlier experiments with ESP8266 networking and relay control, eventually growing into a flexible framework for distributed automation and device coordination.

## System Architecture

NEXUS operates as a distributed network of ESP8266-based devices connected through a standard Wi-Fi network. Each device runs the same firmware but is configured with a specific role using a device identifier.

Devices communicate using UDP broadcast messages, allowing them to automatically discover other nodes on the network and exchange status information.

Unlike traditional IoT architectures that rely heavily on centralized servers or cloud connectivity, NEXUS distributes the logic across devices. Each node can make decisions based on the information received from the network.

Typical architecture example:

User Interface / Controller Device  
↓  
WiFi Network  
↓  
Distributed NEXUS Nodes

Each node can represent different types of devices such as:

Door controllers  
Sensor modules  
Relay actuators  
Interface devices (buttons, intercom systems)

This modular architecture allows the system to grow organically as new devices are added.

## Network Communication

Communication between devices is implemented using UDP broadcast packets. This allows messages to reach every node on the network without requiring prior knowledge of device addresses.

Messages follow a simple structured format:

TYPE|DEVICE|DATA

This format keeps the protocol lightweight and easy to extend while still allowing devices to interpret commands and status updates.

Several types of messages are used to maintain the network and coordinate devices.

### Device Discovery

When a device starts, it announces itself on the network using a discovery message. Other nodes register the device and keep track of its presence.

Example message:

DISCOVERY|DEVICE_NAME|DEVICE_IP

Devices periodically repeat the discovery broadcast to ensure new devices joining the network can detect them.

### Device Confirmation

Once a device detects another node, it confirms the connection and stores its information locally.

Example:

CONFIRM|DEVICE_NAME|DEVICE_IP

This creates a dynamic device list shared by the network.

### Heartbeat Monitoring

To verify that devices remain active, nodes periodically exchange ping and pong messages.

PING|DEVICE_NAME|DEVICE_IP  
PONG|DEVICE_NAME|DEVICE_IP

If a device stops responding for a defined period, it is automatically removed from the internal device list.

This mechanism provides basic network health monitoring.

### Status Synchronization

Each node periodically broadcasts its operational status. This keeps all devices synchronized with the state of the system.

Example:

STATUS|PORTA_A|OPEN  
STATUS|PORTA_B|CLOSED

By continuously sharing state information, devices can react intelligently to changes occurring elsewhere in the network.

### Command Execution

Devices can send commands to other nodes to trigger specific actions.

Example:

OPEN|PORTA_A

When the targeted device receives the command, it executes the corresponding operation locally.

## Device Roles

Although all devices share the same firmware, each node operates under a specific role defined by its device name.

Examples of possible roles include:

Controller devices  
Actuator devices (relays)  
Sensor nodes  
Interface devices such as intercom panels or control buttons

This design simplifies deployment because the same codebase can be reused across different hardware installations.

## Hardware Platform

The current implementation uses ESP8266 microcontrollers, particularly development boards such as NodeMCU.

The NodeMCU platform was chosen due to its integrated Wi-Fi connectivity, sufficient GPIO availability, USB programming interface and stable power regulation.

Typical hardware elements used in the system include:

ESP8266 NodeMCU boards  
Relay modules  
Magnetic door sensors  
Push buttons or switches  
Indicator LEDs

Because the architecture is modular, new hardware types can be integrated easily.

## Automation Logic

One of the core concepts explored in NEXUS is the ability for devices to make decisions based on the state of other devices.

For example, a node controlling a relay can refuse to activate if another device reports a conflicting state. This allows the implementation of safety systems, interlocking mechanisms, and cooperative automation behaviors.

Instead of a centralized controller managing all logic, the network distributes responsibility across nodes.

This makes the system resilient and flexible.

## Reliability and Safety

To prevent inconsistent states caused by communication failures, devices continuously monitor the last time they received updates from other nodes.

If the system loses communication with a critical device, certain actions may be blocked until the connection is restored.

This approach helps prevent dangerous situations in automation systems where multiple actuators must operate under coordinated conditions.

## Potential Applications

Although still an experimental platform, NEXUS can serve as the basis for many types of distributed automation systems.

Examples include:

Access control systems  
Smart building automation  
Distributed relay networks  
Environmental sensor grids  
Industrial monitoring prototypes  
Security and interlock systems

The flexible architecture allows the system to scale from small prototypes to more complex installations.

## Future Development

Several improvements can expand the capabilities of the NEXUS platform.

Possible future directions include:

Web-based monitoring dashboards  
Device configuration interfaces  
Integration with MQTT brokers  
Event logging and diagnostics  
Authentication and network security  
Firmware modularization  
Sensor node expansion  
Mobile interface applications

These additions could transform NEXUS into a more complete distributed automation framework.

## Author

Developed by Davi Han Ko