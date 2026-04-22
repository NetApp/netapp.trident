# delete_volume_snapshotclass role

## Overview

The `delete_volume_snapshotclass` role is part of the **NetApp Trident Validated Content Collection**. It deletes a cluster-scoped `VolumeSnapshotClass` resource by name.

## Features

- Deletes the specified `VolumeSnapshotClass` (cluster-scoped; no namespace).

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `delete_volume_snapshotclass_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `delete_volume_snapshotclass_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `delete_volume_snapshotclass_vol_snapshot_class_specs` | Dict identifying the `VolumeSnapshotClass` to delete. | **Required** (no default) |

Nested key of `delete_volume_snapshotclass_vol_snapshot_class_specs`:

| Key | Description | Default |
|-----|-------------|---------|
| `name` | Name of the `VolumeSnapshotClass` to delete. | **Required** (no default) |

## Example Playbook

```yaml
---
- name: Delete Trident VolumeSnapshotClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    delete_volume_snapshotclass_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    delete_volume_snapshotclass_oc_api_token: "{{ OC_API_TOKEN }}"
    delete_volume_snapshotclass_vol_snapshot_class_specs:
      name: test-ontap-snapshot-class
  roles:
    - delete_volume_snapshotclass
```
