# Create StorageClass Role

## Overview

The `create_storageclass` role is part of the **NetApp Trident Validated Content Collection**. It creates Kubernetes StorageClasses backed by **NetApp Trident** (`csi.trident.netapp.io`) for NFS, NFS FlexGroup, iSCSI, NVMe/TCP, and FCP backends.

## Features

- Creates StorageClasses for the following backends:
  - `ontap-nas` (NFS)
  - `ontap-nas-flexgroup` (NFS FlexGroup)
  - `ontap-san` (iSCSI, NVMe/TCP, FCP)
- Allows per-backend configuration of reclaim policy, volume binding mode, and NFS version.
- Each StorageClass is created only when the corresponding `configure_*` flag is enabled.

## Requirements

* Ansible v2.16.0 or newer
* Python 3.12 or newer
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* NetApp Trident installed and configured in the cluster with matching backend types.

## Role Variables

| Variable                  | Description                                                                                                | Default   |
|---------------------------|------------------------------------------------------------------------------------------------------------|-----------|
| `create_storageclass_oc_api_url`              | OpenShift/Kubernetes API URL.                                                                              |           |
| `create_storageclass_oc_api_token`            | OpenShift/Kubernetes API token.                                                                            |           |
| `create_storageclass_configure_nfs`           | Set to `true` to create the NFS StorageClass.                                                              | `false`   |
| `create_storageclass_configure_nfs_flexgroup` | Set to `true` to create the NFS FlexGroup StorageClass.                                                    | `false`   |
| `create_storageclass_configure_iscsi`         | Set to `true` to create the iSCSI StorageClass.                                                            | `false`   |
| `create_storageclass_configure_fcp`           | Set to `true` to create the FCP StorageClass.                                                              | `false`   |
| `create_storageclass_configure_nvme_tcp`      | Set to `true` to create the NVMe/TCP StorageClass.                                                         | `false`   |
| `create_storageclass_nfs_specs`               | NFS StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`, `nfs_version`).           | See defaults |
| `create_storageclass_nfs_flexgroup_specs`     | NFS FlexGroup StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`, `nfs_version`). | See defaults |
| `create_storageclass_iscsi_specs`             | iSCSI StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`).                        | See defaults |
| `create_storageclass_nvme_tcp_specs`          | NVMe/TCP StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`).                     | See defaults |
| `create_storageclass_fcp_specs`               | FCP StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`).                          | See defaults |

## Example Playbook

```yaml
---
- name: Create NFS StorageClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    create_storageclass_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    create_storageclass_oc_api_token: "{{ OC_API_TOKEN }}"
    create_storageclass_configure_nfs: true
    create_storageclass_nfs_specs:
      sc_name: ontap-nfs-sc
      sc_reclaim_policy: Delete
      sc_volume_binding_mode: Immediate
      nfs_version: "4.1"
  roles:
    - create_storageclass
```
