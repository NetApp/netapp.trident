# Set Default StorageClass Role

## Overview

The `set_default_storageclass` role marks a single Kubernetes StorageClass as the cluster-wide default, clearing the `storageclass.kubernetes.io/is-default-class` annotation from any currently default StorageClass first.

## Features

- Fetches all existing StorageClasses in the cluster.
- Removes the default annotation from any existing default StorageClass.
- Sets the specified StorageClass as the new default.

## Requirements

* Ansible v2.16.0 or newer
* Python 3.12 or newer
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* The target StorageClass must already exist (typically created by the `create_storageclass` role).

## Role Variables

| Variable                  | Description                                                                                              | Default        |
|---------------------------|----------------------------------------------------------------------------------------------------------|----------------|
| `oc_api_url`              | OpenShift/Kubernetes API URL.                                                                            |                |
| `oc_api_token`            | OpenShift/Kubernetes API token.                                                                          |                |
| `default_sc`              | Name of the StorageClass to mark as the cluster default. Must match one of the created StorageClasses.   | `ontap-nfs-sc` |
| `configure_nfs`           | Set to `true` if the NFS StorageClass is in scope for the default-class check.                           | `false`        |
| `configure_nfs_flexgroup` | Set to `true` if the NFS FlexGroup StorageClass is in scope for the default-class check.                 | `false`        |
| `configure_iscsi`         | Set to `true` if the iSCSI StorageClass is in scope for the default-class check.                         | `false`        |
| `configure_fcp`           | Set to `true` if the FCP StorageClass is in scope for the default-class check.                           | `false`        |
| `configure_nvme_tcp`      | Set to `true` if the NVMe/TCP StorageClass is in scope for the default-class check.                      | `false`        |

At least one `configure_*` flag must be `true` for this role to run.

## Example Playbook

```yaml
---
- name: Set the Trident NFS StorageClass as the default
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    configure_nfs: true
    default_sc: ontap-nfs-sc
  roles:
    - set_default_storageclass
```
