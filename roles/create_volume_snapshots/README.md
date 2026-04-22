# create_volume_snapshots role

## Overview

The `create_volume_snapshots` role creates `VolumeSnapshot` resources for existing PVCs in a namespace, referencing an existing `VolumeSnapshotClass`.

## Features

- Lists all existing PVCs in the target namespace to validate sources before creating snapshots.
- Creates a `VolumeSnapshot` per entry in `create_volume_snapshots_volume_snapshot_specs` whose `pvc_name` exists.
- Prints a warning and skips snapshot creation for entries whose `pvc_name` does not exist.

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* Source PVCs already provisioned in `create_volume_snapshots_pvc_namespace` (typically via the `create_pvcs` role).
* `VolumeSnapshotClass` already created (typically via the `create_volume_snapshotclass` role).

## Role Variables

| Variable                        | Description                                                                                 | Default                                  |
|---------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------|
| `create_volume_snapshots_oc_api_url`                    | OpenShift/Kubernetes API URL.                                                               | `https://api.example.openshift.com:6443` |
| `create_volume_snapshots_oc_api_token`                  | OpenShift/Kubernetes API token.                                                             | `<your_api_token>`                       |
| `create_volume_snapshots_pvc_namespace`                 | Namespace where the source PVCs live and snapshots will be created.                         | `test-project`                           |
| `create_volume_snapshots_vol_snapshot_class_specs.name` | Name of an existing `VolumeSnapshotClass` to reference.                                     | `test-ontap-snapshot-class`              |
| `create_volume_snapshots_volume_snapshot_specs`         | List of dicts with `snapshot_name` and `pvc_name`.                                          | See defaults                             |

## Example Playbook

```yaml
---
- name: Create Trident VolumeSnapshots
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    create_volume_snapshots_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    create_volume_snapshots_oc_api_token: "{{ OC_API_TOKEN }}"
    create_volume_snapshots_pvc_namespace: test-project
    create_volume_snapshots_vol_snapshot_class_specs:
      name: test-ontap-snapshot-class
    create_volume_snapshots_volume_snapshot_specs:
      - { snapshot_name: nfs-pvc-snapshot-1, pvc_name: test-nfs-pvc-1 }
  roles:
    - create_volume_snapshots
```
