---
title: Simulate pressure scene
sidebar_label: Simulate pressure scene
---

This paper focuses on how to use Chaosd to simulate pressure scenarios.This feature generates CPU or memory pressure on the host using [stress-ng](https://wiki.ubuntu.com/Kernel/Reference/stress-ng) to support the creation of stress experiments in command-line or service mode.

## Create pressure experiments using command-line mode

This section describes how to create pressure experiments in command-line mode.

Before creating pressure experiments, you can run the following commands to view Chaosd supported stress test types：

```bash
chaosd attack address --help
```

Output shown below：

```bash
Stress attack related orders

Usage:
  chaosd attack stress [command]

Available Commands:
  cpu continued stress CPU out
  mem continued to address virtual memory out

Flags:
  -h, --help help help for stress

Global Flags:
      --log-level define the log level of chaosd, The value can be 'debug', 'info', 'warn' and 'error'

Use "chaosd attack stress [command] --help" for more information about a order.
```

Currently Chaosd supports creating CPU pressure experiments and memory pressure experiments.

### Simulate CPU pressure scene

Run the following command to view the configuration supported by the CPU Pressure Scene：

```bash
chaosd attack address cpu --help
```

Output shown below：

```bash
Continuing address CPU out

Usage:
  chaosd attack cpu [options] [flags]

Flags:
  -h, --help help help for cpu
  -l, --load int Load speciations P per cent load per CPU worker. 0 is effectively a sleep (no load) and 100 is full loading. (default 10)
  -o, --options strings extended stress-ng options.
  -w, --workers int workers specializes N workers to apply the story. (default 1)

Global Flags:
      --log-level string the log level of chaosd, The value can be 'debug', 'info', 'warn' and 'error'
```

The associated configuration is described below in the table：

| Configuration Item | Configure abbreviations | Note                                                                                         | Value                                      |
|:------------------ |:----------------------- |:-------------------------------------------------------------------------------------------- |:------------------------------------------ |
| load               | l                       | Specify the percentage of CPU load using each worker.If 0, an empty load; a full load of 100 | int type, range 0 to 100, default value 10 |
| Workers            | w                       | Specify the number of walkers used to generate CPU pressure                                  | int type, default is 1                     |
| Options            | o                       | Stress-ng other parameter settings, typically not configured                                 | string type, default value is ""           |

### Simulate memory pressure scene

运行以下命令可查看模拟内存压力场景支持的配置：

```bash
chaosd attack address mem --help
```

Output shown below：

```bash
continued virtual memory out

Usage:
  chaosd attack address mem [options] [flags]

Flags:
  -h, --help help help for mem
  -o, --options strings extended stress-ng options.
  -s, --size string Size specialities N bytes consumer per vm worker, default is the total available memory. One can specify the size as % of total available memory or in units of B, KB/KiB, MB/MiB, GB/GiB, TB/TiB.
  -w, --workers int workers specializes N workers to apply the story. (default 1)

Global Flags:
      --log-level string the log level of chaosd, The value can be 'debug', 'info', 'warn' and 'error'
```

Simulation of memory pressure associated with configuration is shown below in table：

| Configuration Item | Configure abbreviations | Note                                                           | Value                                                                                                                             |
|:------------------ |:----------------------- |:-------------------------------------------------------------- |:--------------------------------------------------------------------------------------------------------------------------------- |
| size               | s                       | Specify the size of memory per vm walker                       | Use unit B,KB/KiB,MB/MiB,GB/GiB,TB/TiB is supported to set the usage memory size.If not set, use all available memory by default. |
| Workers            | w                       | Specify the number of walkers used to generate memory pressure | int type, default is 1                                                                                                            |
| Options            | o                       | Stress-ng other parameter settings, typically not configured   | string type, default value is ""                                                                                                  |

### Use Example

Simulate CPU pressure：

```bash
chaosd attack cpu --workers 2 --load 10
```

Output shown below：

```bash
[2021/05/12 03:38:33.698 +00:00] [INFO] [stress.go:66] ["ressors normalize"] [arguments=" --cpu 2 --cpu-load 10"]
[2021/05/12 03:38:33.72 +00:00] [INFO] [stress. o:82] ["Start stress-ng process successfully"] [command="/usr/bin/stres-ng --cpu 2 --cpu-load 10"] [Pid=27483]
Attack stress cpu sucessfully, uid: 4f33b2d4-aeee6-43ca-9c43-0f12867e5c9c
```

Simulate generating memory pressure：

```bash
chaosd attack address mem --workers 2 --size 100M
```

Output shown below：

```bash
[2021/05/12 03:37:19.63 +00:00] [INFO] [stress.go:66] ["stressors normalize"] [arguments=" --vm 2 --vm-keep-vm-bytes 100000"]
[2021/05/12 03:37:37:19.654 +00:00] [INFO] [stress. o:82] ["Start stress-ng process successfully"] [command="/usr/bin/stres-ng --vm 2 --vm-bytes 100000"] [Pid=26799]
Attack stress mem successfully, uid: c2bff2f5-3aac-4ac-b7a6-322946ae6f13
```

When running the experiment, care is taken to save your experiment's uid information.在不需要模拟压力场景时，使用 `recover` 命令来结束 uid 对应的实验：

```bash
chaosd recover c2bff2f5-3aac-4ac-b7a6-322946ae6f13
```

Output shown below：

```bash
Recover c2bff2f5-3aac-4ace-b7a6-322946ae6f13 succesfully
```

## Create pressure experiments using service mode

(To be added)
