# Security-Monitoring-and-Logging-using-Wazuh
# Perform-Bruteforce-attack-with-kali-Linux-and-monitor-endpoint-with-Wazuh-agent

## Overview
A small, controlled lab demonstrating how **Wazuh** can detect authentication brute-force activity against a **Windows 10** endpoint. The lab uses a dedicated attacker host (Kali Linux) to execute a Brute-force attack on windows 10 system and detects the repeated failed logon attempts using Wazuh. The environment is intended for defensive training and analysis in an isolated lab. 

## Objectives
- Demonstrate how to install and register a Wazuh agent on Windows 10.
- Generate reproducible failed logon events on the endpoint (controlled simulation).
- Observe Wazuh alerts and collected Windows event data related to failed authentication.
- Practice investigation steps and confirm detection using Windows Event IDs and Wazuh alerts.

## Lab Topology
- **Windows 10** — Endpoint with Wazuh agent installed (target of simulated authentication attempts).
- **Wazuh Manager** — Central manager collecting agent data and generating alerts.
- **Kali Linux** — Isolated attacker machine used only for controlled simulations in this lab.
> **Important:** Perform all testing in an isolated lab (VMs on a private network) and only when you have written permission to test the target systems.

## Prerequisites
- Virtual machines or physical hosts isolated from production networks.
- Wazuh manager (version compatible with your agent).
- Wazuh agent for Windows installed on the Windows 10 endpoint.
- A local account on Windows for testing (do not test against real user accounts).
- A non-destructive simulation script placed in `/simulations` (this repo’s script generates repeated, controlled failed logon events — no instructions to perform real attacks are provided here).

## High-level setup steps
1. Deploy the VMs (Windows 10, Wazuh manager, Kali Linux) on an isolated network.
2. Install and configure Wazuh manager according to official Wazuh docs.
3. Install the **Wazuh agent** on the Windows 10 endpoint and register it with the manager.
4. Verify agent connectivity in the Wazuh manager UI.
5. Place or enable the provided simulation (see `/simulations`) that generates controlled authentication failures on the Windows endpoint.
6. Use the simulation to create failed logon events while monitoring Wazuh alerts.

> This README intentionally omits offensive, step-by-step attack commands. Use only the provided simulation scripts and never test against systems you do not own or have explicit permission to test.

## What to check in Wazuh
- **Wazuh Alerts / Rules:** Look for alerts related to repeated failed logins or suspicious authentication patterns.
- **Windows Event Logs (collected by Wazuh):**
  - **Event ID 4625** — An account failed to log on (useful to identify failed authentication attempts).
  - **Event ID 4624** — Successful logon (for context/comparison).
- **Agent logs** on the Windows host (`C:\Program Files (x86)\ossec-agent\...` or the path used by your agent) for any ingestion errors.

## Expected alerts / examples
> Example (sanitized) Windows Event (simplified):

