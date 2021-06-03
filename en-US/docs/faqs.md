---
title: FAQ FAQ
sidebar_label: FAQ FAQ
---

## FAQ FAQ

### Q: If I do not have Kubernetes clusters employed, can I use Chaos Mosh to create chaos experiments?

No, you can not use Chaos Mesh in this case. But still you can run your experiences using command. Refer to [Command Line Usages of Chaos](https://github.com/pingcap/tipocket/blob/master/doc/command_line_chaos.md) for details.

### Q: I have lost Chaos Mesh and created PodChaos experiences successwell, but I still failed in creating NetworkChaos/TimeChaos Experiment. The log is visible below:

```
2020-06-18T01:05:26.207Z ERROR controllers. imeChaos failed to apply chaos on all periods {"conciliator": "timechaos", "Error": "rpc error: code = Unavavailable desc = connection error: desc = \"transport: Error while dividing dial tcp xx. x.xxx:xxxx: connect: connection refused\""}
```

You can try using the parameter: `hostNetwork`as shown below:

```
# vim helm/chaos-mesh/values.yaml, change hostNetwork from false to true
hostNetwork: true
```

### Q: I just saw `ERROR: failed to get cluster internal kubeconconfig: command "docker exec --privileged kind-control-plane cat /etc/kubernetes/admin. onf" failed with error: exit status 1` when installing Chaos Mesh with ind. How to fix it?

You can try the following order to fix it:

```
type delete cluster
```

then uploy agin.

## Debug

### Q: Experience not working after chaos is applied

You can debug as described below:

Execute `kubtl descripbe` to check the specified Chaos experience resource.

- If there are `NextStart` and `NextRecover` fields under `spec`, then the chaos will be rigged after `NextStart` is executed.

- If there are no `NextStart` and `NextRecover`fields in `spec`, run the following command to get controller manager's log and see whether there are errors in it.

  ```bash
  kubtl logs -n chaos-controlling chaos-manager-xxxx (replace this with the name of the controller-manager) | grep "ERROR"
  ```

  For error message `no pod is selected`, run the following command to show the labels and check if the selector is desire.

  ```bash
  kubtl get Methods -n yourNamespace --show-labels
  ```

If the above steps cannot solve the problem or you count other related errors in controller's log, [file an issue](https://github.com/chaos-mesh/chaos-mesh/issues) or message us in the #project-chaos-mesh channel in the [CNF Slack](https://join.slack.com/t/cloud-native/shared_invite/zt-fyy3b8up-qHeDNVqbz1j8HDY6g1cY4w) workspace.

## IOChaos

### Q: Running chaosfs residecar container failed, and log shows `pid file found, ensure docker is not running or delete /tmp/fuse/pid`

The chaosfs sidecar content is continuously restored, and you might see the following logs at the current residecar container:

```
2020-01-19T06:30:56.629Z INFO chaos-daemon Init hookfs
2020-01-19T06:30:56. 30Z ERROR chaos-daemon failed to create pid file {"error": "pid file found", ensure docker is not running or delete /tmp/fuse/pid"}
github. om/go-lobr/zapr.(*zapLogger).Error
```

- **Cause**: Chaos Mesh uses Fuse to inject I/O failures. It fails if you specify an existing directory as the source path for chaos. This often happens when you try to reuse a persistent volume (PV) with the `Retain` recalim policy to request a PersistentVolumes (PVC) resource.
- **Solvout**: In this case, use the following order to change the recalim policy to `Delete`:

```bash
kubtl patch pv <your-pv-name> -p '{spec":{persentVolumeReclaim Policy":"Delete"}}
```

## Install

### Q: While trying to install chaos-mesh in OpenShift, tripped over problems regarding authorization.

Message lost like this:

```bash
Error creating: Methods "chaos-daemon-" is forbidden: not able
 to validate against any security context constraint: [spec. ecurityContext.hostNetwork:
 Invalid value: true: Host network is not allowed to be used speci.securityContext. ostPID:
 Invalid value: true: Host PID is not allowed to be used speci.securityContext. ostIPC:
 Invalid value: true: Host IPC is not allowed to be used securityContext. unAsUser:
 Invalid value: "hostPath": hostPath volumes are not allowed to be used. ontakers[0].securityContext.volures[1]:
 Invalid value: true: Host network is not allowed to be used speci.containers[0].securityContext.containers[0]. osport:
 Invalid value: 31767: Host ports are not allowed to be used speci.containers[0].securityContext. ostPID:
 Invalid value: true: Most PID is not allowed to be used speci.containers[0].securityContext.hostIPC:
...]
```

You need to do privileged scc to default.

```bash
ad policy add-scc-to-user prived -n chaos-testing -z default
```
