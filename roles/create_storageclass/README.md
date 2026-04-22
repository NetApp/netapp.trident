# Create storage class role

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

| Variable                                    | Description                                                                           | Default                                |
|---------------------------------------------|---------------------------------------------------------------------------------------|----------------------------------------|
| `oc_api_url`                                | OpenShift/Kubernetes API URL.                                                         |                      |
| `oc_api_token`                              | OpenShift/Kubernetes API token.                                                       |                      |
| `configure_nfs`                             | Whether to create the NFS StorageClass.                                               | **Required** (no default)              |
| `configure_nfs_flexgroup`                   | Whether to create the NFS FlexGroup StorageClass.                                     | **Required** (no default)              |
| `configure_iscsi`                           | Whether to create the iSCSI StorageClass.                                             | **Required** (no default)              |
| `configure_fcp`                             | Whether to create the FCP StorageClass.                                               | **Required** (no default)              |
| `configure_nvme_tcp`                        | Whether to create the NVMe/TCP StorageClass.                                          | **Required** (no default)              |
| `nfs_specs`                                 | NFS StorageClass spec (`sc_name`, `sc_reclaim_policy`, `sc_volume_binding_mode`, `nfs_version`). | See defaults           |
| `nfs_flexgroup_specs`                       | NFS FlexGroup StorageClass spec.                                                      | See defaults                           |
| `iscsi_specs`                               | iSCSI StorageClass spec.                                                              | See defaults                           |
| `nvme_tcp_specs`                            | NVMe/TCP StorageClass spec.                                                           | See defaults                           |
| `fcp_specs`                                 | FCP StorageClass spec.                                                                | See defaults                           |

## Example Playbook

```yaml
---
- name: Create NFS StorageClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    nfs_specs:
      sc_name: ontap-nfs-sc
      sc_reclaim_policy: Delete
      sc_volume_binding_mode: Immediate
      nfs_version: "4.1"
  roles:
    - create_storageclass
```
