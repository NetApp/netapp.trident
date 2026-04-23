# delete_storageclass role

## Overview

The `delete_storageclass` role is part of the **NetApp Trident Validated Content Collection**. It deletes Kubernetes/OpenShift `StorageClass` resources (cluster-scoped) previously created for NetApp Trident backends (NFS, NFS FlexGroup, iSCSI, NVMe/TCP, FCP).

## Features

- Deletes the per-backend StorageClass referenced by `*_specs.sc_name` when the corresponding `configure_*` flag is set to `true`.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `delete_storageclass_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `delete_storageclass_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `delete_storageclass_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `delete_storageclass_configure_nfs` | Set to `true` to delete the NFS StorageClass. | `false` |
| `delete_storageclass_configure_nfs_flexgroup` | Set to `true` to delete the NFS FlexGroup StorageClass. | `false` |
| `delete_storageclass_configure_iscsi` | Set to `true` to delete the iSCSI StorageClass. | `false` |
| `delete_storageclass_configure_fcp` | Set to `true` to delete the FCP StorageClass. | `false` |
| `delete_storageclass_configure_nvme_tcp` | Set to `true` to delete the NVMe/TCP StorageClass. | `false` |
| `delete_storageclass_nfs_specs` | Dict whose `sc_name` is the NFS StorageClass to delete. | **Required** when `delete_storageclass_configure_nfs` is `true` |
| `delete_storageclass_nfs_flexgroup_specs` | Dict whose `sc_name` is the NFS FlexGroup StorageClass to delete. | **Required** when `delete_storageclass_configure_nfs_flexgroup` is `true` |
| `delete_storageclass_iscsi_specs` | Dict whose `sc_name` is the iSCSI StorageClass to delete. | **Required** when `delete_storageclass_configure_iscsi` is `true` |
| `delete_storageclass_nvme_tcp_specs` | Dict whose `sc_name` is the NVMe/TCP StorageClass to delete. | **Required** when `delete_storageclass_configure_nvme_tcp` is `true` |
| `delete_storageclass_fcp_specs` | Dict whose `sc_name` is the FCP StorageClass to delete. | **Required** when `delete_storageclass_configure_fcp` is `true` |

Each `*_specs` dict accepts the following key:

| Key | Description | Default |
|-----|-------------|---------|
| `sc_name` | Name of the StorageClass to delete (cluster-scoped). | **Required** (no default) |

> Note: At least one `delete_storageclass_configure_*` flag must be `true`.

## Example Playbook

```yaml
---
- name: Delete NFS StorageClass
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    delete_storageclass_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    delete_storageclass_oc_api_token: "{{ OC_API_TOKEN }}"
    delete_storageclass_configure_nfs: true
    delete_storageclass_nfs_specs:
      sc_name: ontap-nfs-sc
  roles:
    - delete_storageclass
```
