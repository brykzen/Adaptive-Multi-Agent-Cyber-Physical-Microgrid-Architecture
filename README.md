# Resilient Industrial Microgrid — Multi-Agent Analysis Architecture

![Status](https://img.shields.io/badge/status-architecture%20phase-yellow?style=flat-square)
![Focus](https://img.shields.io/badge/focus-cyber--physical%20systems-blue?style=flat-square)
![Standard](https://img.shields.io/badge/standard-ISA--95%20%7C%20IEC%2062443%20%7C%20IEC%2061508-1f6feb?style=flat-square)

---

## Project Status

This project is currently in the **architectural design phase**. 

No implementation has been completed yet. This repository serves as a **system blueprint and development roadmap** for a multi-agent industrial microgrid supervision framework. It bridges theoretical research with a future practical deployment in virtualized OT environments.

---

## Project Concept

Modern industrial infrastructures require a correlation layer capable of distinguishing between physical transients and cyber-protocol manipulation. This architecture introduces a **Context-Aware Coordination Layer** where specialized agents monitor the plant's state. Instead of a fixed hierarchy, authority is dynamically assigned by a **Conflict Resolver** based on the detected integrity of the system.

---

## Cyber-Physical Architecture (Purdue Model)

<p align="center">
  <img src="./docs/images/purdue-model.svg"
       alt="Purdue Model Architecture"
       width=500>
</p>

### Functional Layer Mapping

| Layer | Component | Functional Role |
| :--- | :--- | :--- |
| **L4 & L5 - Enterprise** | ERP & Cloud Systems | Strategic planning, logistics, and high-level remote analytics. |
| **DMZ - Security** | Cyber Agent & Mirror Broker | Traffic inspection via Mirror Broker and behavioral anomaly detection. |
| **L3 - Operations** | Conflict Resolver & Optimization Agent | Coordination, decision arbitration, and process optimization (MES/Historian). |
| **L2 - Supervisory** | SCADA / HMI & Safety Agent | Real-time monitoring, edge data orchestration, and safety constraint enforcement. |
| **L1 - Control** | SoftPLC (OpenPLC) | Deterministic execution of control loops and setpoint processing via OPC UA. |
| **L0 - Physical** | Python Digital Twin Models | Mathematical modeling of asset dynamics feeding the cyber-physical system. |

---

## Design Principles & Decisions

### Non-Interference Principle
Intelligence operates exclusively in an advisory capacity. No agent writes directly to the control bus. The SCADA/PLC chain retains executive authority to ensure deterministic safety.

### Semantic Data Orchestration (UNS)
A Unified Namespace (UNS) decouples data producers from consumers. This allows parallel observation by multiple agents without increasing controller CPU load or network jitter.

### Asymmetric Authority & Conflict Resolution
Authority is context-dependent. The Conflict Resolver weights agent recommendations based on a **Sensor Trust Score**. If a cyber-anomaly is detected, the system can invalidate corrupted inputs and reassign authority to the Safety Agent.

---

## Implementation Strategy: Proxmox-Based Deployment

The architecture is designed for a **software-defined industrial environment** hosted on **Proxmox VE**, prioritizing network isolation and resource efficiency:

* **Network Core:** **OPNsense** virtual appliance manages inter-VLAN routing and strict firewall rules between Purdue layers.
* **Micro-Segmentation:** **Open vSwitch (OVS)** handles logical separation at the bridge level, ensuring isolation for critical control traffic.
* **Lightweight Control:** PLCs and Agents deployed as **Docker containers** or **LXC**, allowing for high-density microgrid emulation with minimal overhead.
* **Cyber-DMZ:** Isolated zone utilizing traffic mirroring for out-of-band behavioral analysis.

---

## Multi-Agent Ecosystem

* **Safety Agent (L2):** Monitors the Safe Operating Area (SOA) and enforces physical constraints.
* **Optimization Agent (L3):** Handles efficiency, cost-of-service, and load forecasting.
* **Cyber Agent (DMZ):** Performs behavioral analysis of industrial protocols (OPC UA/MQTT) to detect spoofing or unauthorized movement.

---

## Validation Roadmap

Performance evaluation will compare a **Baseline Mode** (Standard PID control) against the **Active Resilience Mode** (Multi-agent system).

**Primary Metrics:**
* **MTTR:** Mean Time to Recovery after fault or cyber-attack.
* **Setpoint Deviation:** Stability analysis under adversarial conditions.
* **Detection Precision:** Accuracy and false-positive rates of agent observations.

---

## Tech Stack Summary

* **Virtualization:** Proxmox VE (LXC, Docker & VM orchestration)
* **Network & Security:** OPNsense Firewall & Open vSwitch
* **Control:** OpenPLC (IEC 61131-3)
* **SCADA:** Ignition / HMI layer
* **Communication:** OPC UA, MQTT Sparkplug B, Mosquitto
* **Simulation:** Python-based Digital Twin modeling
