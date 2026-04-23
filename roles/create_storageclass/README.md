# create_storageclass role

## Overview

The `create_storageclass` role is part of the **NetApp Trident Validated Content Collection**. It creates Kubernetes/OpenShift `StorageClass` resources backed by **NetApp Trident** (`csi.trident.netapp.io`) for NFS, NFS FlexGroup, iSCSI, NVMe/TCP, and FCP backends.

## Features

- Creates one StorageClass per enabled backend:
  - `ontap-nas` (NFS)
  - `ontap-nas-flexgroup` (NFS FlexGroup)
  - `ontap-san` with `sanType: iscsi` (iSCSI)
  - `ontap-san` with `sanType: nvme` (NVMe/TCP)
  - `ontap-san` with `sanType: fcp` (FCP)
- Allows per-backend configuration of reclaim policy, volume binding mode, and (for NAS) NFS version.
- Each StorageClass is created only when its corresponding `configure_*` flag is set to `true`.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.
* NetApp Trident installed and configured in the cluster with matching backend types.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `create_storageclass_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `create_storageclass_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `create_storageclass_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `create_storageclass_configure_nfs` | Set to `true` to create the NFS StorageClass. | `false` |
| `create_storageclass_configure_nfs_flexgroup` | Set to `true` to create the NFS FlexGroup StorageClass. | `false` |
| `create_storageclass_configure_iscsi` | Set to `true` to create the iSCSI StorageClass. | `false` |
| `create_storageclass_configure_fcp` | Set to `true` to create the FCP StorageClass. | `false` |
| `create_storageclass_configure_nvme_tcp` | Set to `true` to create the NVMe/TCP StorageClass. | `false` |
| `create_storageclass_nfs_specs` | NFS StorageClass spec dict (see nested keys below). | **Required** when `create_storageclass_configure_nfs` is `true` |
| `create_storageclass_nfs_flexgroup_specs` | NFS FlexGroup StorageClass spec dict. | **Required** when `create_storageclass_configure_nfs_flexgroup` is `true` |
| `create_storageclass_iscsi_specs` | iSCSI StorageClass spec dict. | **Required** when `create_storageclass_configure_iscsi` is `true` |
| `create_storageclass_nvme_tcp_specs` | NVMe/TCP StorageClass spec dict. | **Required** when `create_storageclass_configure_nvme_tcp` is `true` |
| `create_storageclass_fcp_specs` | FCP StorageClass spec dict. | **Required** when `create_storageclass_configure_fcp` is `true` |

Each `*_specs` dict accepts the following keys:

| Key | Description | Default |
|-----|-------------|---------|
| `sc_name` | Name of the StorageClass to create. | **Required** (no default) |
| `sc_volume_binding_mode` | Volume binding mode (`Immediate` or `WaitForFirstConsumer`). | **Required** (no default) |
| `sc_reclaim_policy` | Reclaim policy for the StorageClass (`Delete` or `Retain`). | `Retain` |
| `nfs_version` | NFS protocol version (for example, `"3"`, `"4.1"`). Applies only to NFS and NFS FlexGroup. | **Required** for NFS / NFS FlexGroup |

> Note: At least one `create_storageclass_configure_*` flag must be `true`.

## Example Playbook

```yaml
---
- name: Create Trident StorageClasses
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
