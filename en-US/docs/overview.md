---
slug: /
title: Chaos Mesh Overview
sidebar_label: Chaos Mesh Overview
---

This document describes the concept, usage scenarios, core strengths, and the architecture overview of Chaos Mesh.

## Chaos Mesh Overview

Chaos Mesh is an open source cloud-native chaos engineering platform that provides a wealth of fault simulation types, and has a powerful fault scenario orchestration capability. It is convenient for users to simulate various abnormalities that might occur in the real world during development, testing and the production environment, help users find potential problems in the system. Chaos Mesh offers a perfect visualization operation designed to lower the threshold for a Chaos engineering project. Users can easily design their chaos scenarios on the Web UI interface, and monitor the status of the chaos experiments.

## Core strengths

As the industry's leading Chaos testing platform, Chaos Mesh has the following core strengths:

- Stable core capabilities: Chaos Mesh originated from the core testing platform of [TiDB](https://github.com/pingcap/tidb), and inherited a lot of TiDB's existing test experience from its initial release.
- Fully authenticated: Chaos Mesh is used by numerous companies and organizations, such as Tencent and Meituan; It is also used in testing systems of many well-known distributed systems, such as Apache APISIX and RabbitMQ.
- An easy-to-use system: Chaos Mesh makes full use of automation and performs graphical operations and Kubernetes-based usage.
- Cloud-native: Chaos Mesh supports Kubernetes environment and provides powerful automation.
- Rich failure simulation scenarios: Chaos Mesh covers almost most of the scenarios of basic failure simulations in the distributed testing system.
- Flexible experiment orchestration capabilities: Users can design their own Chaos experiment scenarios using the platform, including multiple mixing experiments, and application status checks.
- High security: Chaos Mesh has multiple layers of security control design and provides high security.
- An active community: Chaos Mesh is a world-renowned open source mixing testing platform and CNCF Open Source Foundation Incubation project.
- Powerful scalability: Chaos Mesh provides full scalability for fault test type extension and function extension.

## Architecture Overview

Chaos Mesh is built on Kubernetes CRD (Custom Resource Definition). To manage different chaos experiments, Chaos Mesh defines multiple CRD types according to different fault types and implements separate Controllers for different CRD objects. Chaos Mesh primarily contains three components:

- **Chaos Dashboard**: The visualization component of Chaos Mesh. Chaos Dashboard provides a set of user-friendly web interfaces through which users can manipulate and observe chaos experiments. At the same time, Chaos Dashboard also provides an RBAC permission management mechanism.
- **Chaos Controller Manager**: The core logical component of Chaos Mesh. Chaos Controller Manager is primarily responsible for the scheduling and management of chaos experiments. This component contains several CRD Controllers, such as Workflow Controller, Scheduler Controller, and Controllers of various types of failures.
- **Chaos Daemon**: The main executive component. Chaos Daemon runs in the [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)  mode and has Privileged permissions by default (which can be disabled). This component mainly interferes with specific network devices, file systems, kernels, etc. by hacking into the target Pod Namespace.

![Architecture](img/architecture.png)

As shown in the above image, the overall architecture of Chaos Mesh can be divided into three parts from top to bottom:

- User input and observation: User input reaches the Kubernetes API Server starting with a user operation (User). Users do not directly interact with the Chaos Mesh Controller, and all user operations will eventually be reflected in a Chaos resource change (such as the change of NetworkChaos resource).
- Monitor resource changes, schedule Workflow, and carry out chaos experiments: The Controller of Chaos Mesh only accepts events from the Kubernetes API Server. This event describes the change of a Chaos resource, such as a new Workflow object or the creation of a Chaos object.
- Injection of a specific node failure: The Chaos Daemon component is primarily responsible for accepting commands from the Chaos Controller Manager component, hacking into the target Pod's Namespace, and performing specific fault injections. For example, setting TC network rules, starting the stress-ng process to preempt CPU or memory resources, etc.
