# Delete StorageClass Role

## Overview

The `delete_storageclass` role deletes Kubernetes `StorageClass` resources (cluster-scoped) previously created for NetApp Trident backends (NFS, NFS FlexGroup, iSCSI, NVMe/TCP, FCP).

## Features

- Deletes the per-backend StorageClass referenced by `*_specs.sc_name` when the corresponding `configure_*` flag is `true`.

## Requirements

* Ansible v2.16.0 or newer
* Python 3.12 or newer
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                      | Description                                             | Default                  |
|-------------------------------|---------------------------------------------------------|--------------------------|
| `oc_api_url`                  | OpenShift/Kubernetes API URL.                           |                          |
| `oc_api_token`                | OpenShift/Kubernetes API token.                         |                          |
| `configure_nfs`               | Set to `true` to delete the NFS StorageClass.           | `false`                  |
| `configure_nfs_flexgroup`     | Set to `true` to delete the NFS FlexGroup StorageClass. | `false`                  |
| `configure_iscsi`             | Set to `true` to delete the iSCSI StorageClass.         | `false`                  |
| `configure_fcp`               | Set to `true` to delete the FCP StorageClass.           | `false`                  |
| `configure_nvme_tcp`          | Set to `true` to delete the NVMe/TCP StorageClass.      | `false`                  |
| `nfs_specs.sc_name`           | Name of the NFS StorageClass to delete.                 | `ontap-nfs-sc`           |
| `nfs_flexgroup_specs.sc_name` | Name of the NFS FlexGroup StorageClass to delete.       | `ontap-nfs-flexgroup-sc` |
| `iscsi_specs.sc_name`         | Name of the iSCSI StorageClass to delete.               | `ontap-iscsi-sc`         |
| `nvme_tcp_specs.sc_name`      | Name of the NVMe/TCP StorageClass to delete.            | `ontap-nvme-tcp-sc`      |
| `fcp_specs.sc_name`           | Name of the FCP StorageClass to delete.                 | `ontap-fcp-sc`           |

## Example Playbook

```yaml
---
- name: Delete Trident StorageClasses
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    configure_nfs: true
    nfs_specs:
      sc_name: ontap-nfs-sc
  roles:
    - delete_storageclass
```
