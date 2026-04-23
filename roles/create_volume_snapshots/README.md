# Create VolumeSnapshot Role

## Overview

The `create_volume_snapshots` role is part of the **NetApp Trident Validated Content Collection**. It creates `VolumeSnapshot` resources for existing Trident-provisioned PVCs in a namespace, referencing an existing `VolumeSnapshotClass`.

## Features

- Lists all existing PVCs in the target namespace to validate snapshot sources before creating snapshots.
- Creates a `VolumeSnapshot` per entry in `create_volume_snapshots_volume_snapshot_specs` whose `pvc_name` exists.
- Prints a warning and skips snapshot creation for entries whose `pvc_name` does not exist in the namespace.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.
* Source PVCs already provisioned in `create_volume_snapshots_pvc_namespace` (typically via the `create_pvcs` role).
* A `VolumeSnapshotClass` already created (typically via the `create_volume_snapshotclass` role).

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `create_volume_snapshots_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `create_volume_snapshots_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `create_volume_snapshots_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `create_volume_snapshots_pvc_namespace` | Namespace where the source PVCs live and the snapshots will be created. | **Required** (no default) |
| `create_volume_snapshots_vol_snapshot_class_specs` | Dict identifying the existing `VolumeSnapshotClass` to reference. | **Required** (no default) |
| `create_volume_snapshots_volume_snapshot_specs` | List of dicts describing the snapshots to create (see keys below). | **Required** (no default) |

Nested key of `create_volume_snapshots_vol_snapshot_class_specs`:

| Key | Description | Default |
|-----|-------------|---------|
| `name` | Name of an existing `VolumeSnapshotClass` to reference. | **Required** (no default) |

Each entry in `create_volume_snapshots_volume_snapshot_specs` is a dict with:

| Key | Description | Default |
|-----|-------------|---------|
| `snapshot_name` | Name of the `VolumeSnapshot` resource to create. | **Required** (no default) |
| `pvc_name` | Name of the source PVC in `create_volume_snapshots_pvc_namespace`. | **Required** (no default) |

## Example Playbook

```yaml
---
- name: Create VolumeSnapshots
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
      - { snapshot_name: nfs-pvc-snapshot-2, pvc_name: test-nfs-pvc-2 }
  roles:
    - create_volume_snapshots
```
