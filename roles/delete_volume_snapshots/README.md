# delete_volume_snapshots role

## Overview

The `delete_volume_snapshots` role deletes `VolumeSnapshot` resources by name from a specified Kubernetes namespace.

## Features

- Deletes each `VolumeSnapshot` listed in `delete_volume_snapshots_volume_snapshot_specs` from `delete_volume_snapshots_pvc_namespace`.

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                  | Description                                                       | Default                                  |
|---------------------------|-------------------------------------------------------------------|------------------------------------------|
| `delete_volume_snapshots_oc_api_url`              | OpenShift/Kubernetes API URL.                                     | `https://api.example.openshift.com:6443` |
| `delete_volume_snapshots_oc_api_token`            | OpenShift/Kubernetes API token.                                   | `<your_api_token>`                       |
| `delete_volume_snapshots_pvc_namespace`           | Namespace containing the `VolumeSnapshot` resources to delete.    | `test-project`                           |
| `delete_volume_snapshots_volume_snapshot_specs`   | List of dicts with `snapshot_name` (and optional `pvc_name`).     | See defaults                             |

## Example Playbook

```yaml
---
- name: Delete Trident VolumeSnapshots
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    delete_volume_snapshots_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    delete_volume_snapshots_oc_api_token: "{{ OC_API_TOKEN }}"
    delete_volume_snapshots_pvc_namespace: test-project
    delete_volume_snapshots_volume_snapshot_specs:
      - { snapshot_name: nfs-pvc-snapshot-1, pvc_name: test-nfs-pvc-1 }
  roles:
    - delete_volume_snapshots
```
