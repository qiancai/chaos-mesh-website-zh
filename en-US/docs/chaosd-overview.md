---
title: Chaosd Component Intro
sidebar_label: Chaosd Component Intro
---

## Chaosd Component Intro

[Chaosd](https://github.com/chaos-mesh/chaosd) is a Chaos Engineering Test Tool provided by Chaos Mesh to inject malfunction into the physical environment and provide troubleshooting.

Chaosd has the following core advantages：

- Enter a simple Chaos command with ease of use：to create chaos and manage the experiment.
- The failure type is rich：provides troubleshooter features at different levels and types of the physical machine, including processes, networks, pressures, disks, hosts, and more features are expanding.
- Support multiple patterns：Chaosd can be used both as a command line tool and as a service to meet the needs of different scenarios.

### Support Incident Type

You can use Chaosd to simulate the following failure type：

- Process：provides troubleshooter to process and supports process kills, stops, etc.
- Network：provides troubleshooter network injections, supports increased network delays, dropping, broken packages, etc.
- Pressure：injects pressure into the CPU or memory of the physics.
- Disk：provides troubleshooter troubleshooting to support actions that increase reading and writing disk loads, filling disks, etc.
- Host：provides troubleshooting to the physical machine itself, supports shutdown and more.

For details of how each type of failure is described and used, please refer to the corresponding documentation file.

### Run mode

You can use Chaosd： in the following mode

- Command line mode：uses Chaosd as a command line tool to inject troubleshooter and restore failures.
- Service mode：runs Chaosd as a service in the backend, by sending HTTP requests to inject troubles and restore failures.
