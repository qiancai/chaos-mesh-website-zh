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

### 模拟内存压力场景

运行以下命令可查看模拟内存压力场景支持的配置：

```bash
chaosd attack stress mem --help
```

输出如下所示：

```bash
continuously stress virtual memory out

Usage:
  chaosd attack stress mem [options] [flags]

Flags:
  -h, --help              help for mem
  -o, --options strings   extend stress-ng options.
  -s, --size string       Size specifies N bytes consumed per vm worker, default is the total available memory. One can specify the size as % of total available memory or in units of B, KB/KiB, MB/MiB, GB/GiB, TB/TiB..
  -w, --workers int       Workers specifies N workers to apply the stressor. (default 1)

Global Flags:
      --log-level string   the log level of chaosd, the value can be 'debug', 'info', 'warn' and 'error'
```

模拟内存压力相关配置说明如下表所示：

| 配置项     | 配置缩写 | 说明                           | 值                                                                   |
|:------- |:---- |:---------------------------- |:------------------------------------------------------------------- |
| size    | s    | 指定每个 vm worker 占用内存的大小       | 支持使用单位 B，KB/KiB，MB/MiB，GB/GiB，TB/TiB 来设置占用的内存大小。如果不设置，则默认占用所有可用的内存。 |
| workers | w    | 指定用于生成内存压力的 worker 数量        | int 类型，默认值为 1                                                       |
| options | o    | stress-ng 的其他参数设置，一般情况下不需要配置 | string 类型，默认值为 ""                                                   |

### 使用示例

模拟生成 CPU 压力：

```bash
chaosd attack stress cpu --workers 2 --load 10
```

输出如下所示：

```bash
[2021/05/12 03:38:33.698 +00:00] [INFO] [stress.go:66] ["stressors normalize"] [arguments=" --cpu 2 --cpu-load 10"]
[2021/05/12 03:38:33.702 +00:00] [INFO] [stress.go:82] ["Start stress-ng process successfully"] [command="/usr/bin/stress-ng --cpu 2 --cpu-load 10"] [Pid=27483]
Attack stress cpu successfully, uid: 4f33b2d4-aee6-43ca-9c43-0f12867e5c9c
```

模拟生成内存压力：

```bash
chaosd attack stress mem --workers 2 --size 100M
```

输出如下所示：

```bash
[2021/05/12 03:37:19.643 +00:00] [INFO] [stress.go:66] ["stressors normalize"] [arguments=" --vm 2 --vm-keep --vm-bytes 100000000"]
[2021/05/12 03:37:19.654 +00:00] [INFO] [stress.go:82] ["Start stress-ng process successfully"] [command="/usr/bin/stress-ng --vm 2 --vm-keep --vm-bytes 100000000"] [Pid=26799]
Attack stress mem successfully, uid: c2bff2f5-3aac-4ace-b7a6-322946ae6f13
```

在运行实验时，请注意保存实验的 uid 信息。在不需要模拟压力场景时，使用 `recover` 命令来结束 uid 对应的实验：

```bash
chaosd recover c2bff2f5-3aac-4ace-b7a6-322946ae6f13
```

输出如下所示：

```bash
Recover c2bff2f5-3aac-4ace-b7a6-322946ae6f13 successfully
```

## 使用服务模式创建压力实验

（待补充）
