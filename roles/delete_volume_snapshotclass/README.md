# delete_volume_snapshotclass role

## Overview

The `delete_volume_snapshotclass` role deletes a cluster-scoped `VolumeSnapshotClass` resource by name.

## Features

- Deletes the specified `VolumeSnapshotClass` (cluster-scoped, no namespace).

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                           | Description                                                 | Default                                  |
|------------------------------------|-------------------------------------------------------------|------------------------------------------|
| `delete_volume_snapshotclass_oc_api_url`                       | OpenShift/Kubernetes API URL.                               | `https://api.example.openshift.com:6443` |
| `delete_volume_snapshotclass_oc_api_token`                     | OpenShift/Kubernetes API token.                             | `<your_api_token>`                       |
| `delete_volume_snapshotclass_vol_snapshot_class_specs.name`    | Name of the `VolumeSnapshotClass` to delete.                | `test-ontap-snapshot-class`              |

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
