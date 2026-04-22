# delete_volume_snapshotclass role

## Overview

The `delete_volume_snapshotclass` role deletes a cluster-scoped `VolumeSnapshotClass` resource by name.

## Features

- Deletes the specified `VolumeSnapshotClass` (cluster-scoped, no namespace).

## Requirements

* Ansible 2.15 or newer.
* `kubernetes.core` collection installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                           | Description                                                 | Default                                  |
|------------------------------------|-------------------------------------------------------------|------------------------------------------|
| `oc_api_url`                       | OpenShift/Kubernetes API URL.                               | `https://api.example.openshift.com:6443` |
| `oc_api_token`                     | OpenShift/Kubernetes API token.                             | `<your_api_token>`                       |
| `vol_snapshot_class_specs.name`    | Name of the `VolumeSnapshotClass` to delete.                | `test-ontap-snapshot-class`              |

## Example Playbook

```yaml
---
- name: Delete Trident VolumeSnapshotClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    vol_snapshot_class_specs:
      name: test-ontap-snapshot-class
  roles:
    - delete_volume_snapshotclass
```
