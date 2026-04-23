# delete_volume_snapshots role

## Overview

The `delete_volume_snapshots` role is part of the **NetApp Trident Validated Content Collection**. It deletes `VolumeSnapshot` resources by name from a specified Kubernetes/OpenShift namespace.

## Features

- Deletes each `VolumeSnapshot` listed in `delete_volume_snapshots_volume_snapshot_specs` from `delete_volume_snapshots_pvc_namespace`.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `delete_volume_snapshots_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `delete_volume_snapshots_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `delete_volume_snapshots_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `delete_volume_snapshots_pvc_namespace` | Namespace containing the `VolumeSnapshot` resources to delete. | **Required** (no default) |
| `delete_volume_snapshots_volume_snapshot_specs` | List of dicts describing the snapshots to delete (see keys below). | **Required** (no default) |

Each entry in `delete_volume_snapshots_volume_snapshot_specs` is a dict with:

| Key | Description | Default |
|-----|-------------|---------|
| `snapshot_name` | Name of the `VolumeSnapshot` to delete. | **Required** (no default) |
| `pvc_name` | Name of the source PVC (informational; not used for deletion). | Optional |

## Example Playbook

```yaml
---
- name: Delete VolumeSnapshots
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    delete_volume_snapshots_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    delete_volume_snapshots_oc_api_token: "{{ OC_API_TOKEN }}"
    delete_volume_snapshots_pvc_namespace: test-project
    delete_volume_snapshots_volume_snapshot_specs:
      - { snapshot_name: nfs-pvc-snapshot-1, pvc_name: test-nfs-pvc-1 }
      - { snapshot_name: nfs-pvc-snapshot-2, pvc_name: test-nfs-pvc-2 }
  roles:
    - delete_volume_snapshots
```
