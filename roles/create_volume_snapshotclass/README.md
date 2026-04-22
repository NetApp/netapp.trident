# create_volume_snapshotclass role

## Overview

The `create_volume_snapshotclass` role creates a cluster-scoped `VolumeSnapshotClass` resource driven by NetApp Trident (`csi.trident.netapp.io`).

## Features

- Creates a `snapshot.storage.k8s.io/v1` `VolumeSnapshotClass` using the Trident CSI driver.
- Supports configurable `deletionPolicy` (`Delete` or `Retain`).

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* External snapshotter controller and the `VolumeSnapshotClass` CRD installed.
* NetApp Trident installed.

## Role Variables

| Variable                               | Description                                                                       | Default                                  |
|----------------------------------------|-----------------------------------------------------------------------------------|------------------------------------------|
| `create_volume_snapshotclass_oc_api_url`                           | OpenShift/Kubernetes API URL.                                                     | `https://api.example.openshift.com:6443` |
| `create_volume_snapshotclass_oc_api_token`                         | OpenShift/Kubernetes API token.                                                   | `<your_api_token>`                       |
| `create_volume_snapshotclass_vol_snapshot_class_specs.name`        | Name of the `VolumeSnapshotClass` to create.                                      | `test-ontap-snapshot-class`              |
| `create_volume_snapshotclass_vol_snapshot_class_specs.deletion_policy` | Deletion policy for the `VolumeSnapshotClass` (`Delete` or `Retain`).         | `Delete`                                 |

## Example Playbook

```yaml
---
- name: Create Trident VolumeSnapshotClass
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
