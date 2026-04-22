# delete_volume_snapshots role

## Overview

The `delete_volume_snapshots` role deletes `VolumeSnapshot` resources by name from a specified Kubernetes namespace.

## Features

- Deletes each `VolumeSnapshot` listed in `volume_snapshot_specs` from `pvc_namespace`.

## Requirements

* Ansible 2.15 or newer.
* `kubernetes.core` collection installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                  | Description                                                       | Default                                  |
|---------------------------|-------------------------------------------------------------------|------------------------------------------|
| `oc_api_url`              | OpenShift/Kubernetes API URL.                                     | `https://api.example.openshift.com:6443` |
| `oc_api_token`            | OpenShift/Kubernetes API token.                                   | `<your_api_token>`                       |
| `pvc_namespace`           | Namespace containing the `VolumeSnapshot` resources to delete.    | `test-project`                           |
| `volume_snapshot_specs`   | List of dicts with `snapshot_name` (and optional `pvc_name`).     | See defaults                             |

## Example Playbook

```yaml
---
- name: Delete Trident VolumeSnapshots
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    pvc_namespace: test-project
    volume_snapshot_specs:
      - { snapshot_name: nfs-pvc-snapshot-1, pvc_name: test-nfs-pvc-1 }
  roles:
    - delete_volume_snapshots
```
