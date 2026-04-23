# create_volume_snapshotclass role

## Overview

The `create_volume_snapshotclass` role is part of the **NetApp Trident Validated Content Collection**. It creates a cluster-scoped `VolumeSnapshotClass` resource driven by **NetApp Trident** (`csi.trident.netapp.io`), which is consumed by `VolumeSnapshot` resources to take CSI snapshots of Trident-provisioned PVCs.

## Features

- Creates a `snapshot.storage.k8s.io/v1` `VolumeSnapshotClass` using the Trident CSI driver.
- Supports a configurable `deletionPolicy` (`Delete` or `Retain`).

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.
* External snapshotter controller and the `VolumeSnapshotClass` CRD installed in the cluster.
* NetApp Trident installed.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `create_volume_snapshotclass_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `create_volume_snapshotclass_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `create_volume_snapshotclass_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `create_volume_snapshotclass_vol_snapshot_class_specs` | Dict describing the `VolumeSnapshotClass` to create (see nested keys below). | **Required** (no default) |

Nested keys of `create_volume_snapshotclass_vol_snapshot_class_specs`:

| Key | Description | Default |
|-----|-------------|---------|
| `name` | Name of the `VolumeSnapshotClass` to create (cluster-scoped). | **Required** (no default) |
| `deletion_policy` | Deletion policy for the `VolumeSnapshotClass` (`Delete` or `Retain`). | `Delete` |

## Example Playbook

```yaml
---
- name: Create VolumeSnapshotClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    create_volume_snapshotclass_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    create_volume_snapshotclass_oc_api_token: "{{ OC_API_TOKEN }}"
    create_volume_snapshotclass_vol_snapshot_class_specs:
      name: test-ontap-snapshot-class
      deletion_policy: Delete
  roles:
    - create_volume_snapshotclass
```
