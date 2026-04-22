# delete_storageclass role

## Overview

The `delete_storageclass` role deletes Kubernetes `StorageClass` resources (cluster-scoped) previously created for NetApp Trident backends (NFS, NFS FlexGroup, iSCSI, NVMe/TCP, FCP).

## Features

- Deletes the per-backend StorageClass referenced by `*_specs.sc_name` when the corresponding `configure_*` flag is `true`.

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                     | Description                                                          | Default                                  |
|------------------------------|----------------------------------------------------------------------|------------------------------------------|
| `oc_api_url`                 | OpenShift/Kubernetes API URL.                                        | `https://api.example.openshift.com:6443` |
| `oc_api_token`               | OpenShift/Kubernetes API token.                                      | `<your_api_token>`                       |
| `configure_*`                | Backend enable flags (`configure_nfs`, `configure_nfs_flexgroup`, `configure_iscsi`, `configure_nvme_tcp`, `configure_fcp`). | **Required** (no default) |
| `nfs_specs.sc_name`          | Name of the NFS StorageClass to delete.                              | `ontap-nfs-sc`                           |
| `nfs_flexgroup_specs.sc_name`| Name of the NFS FlexGroup StorageClass to delete.                    | `ontap-nfs-flexgroup-sc`                 |
| `iscsi_specs.sc_name`        | Name of the iSCSI StorageClass to delete.                            | `ontap-iscsi-sc`                         |
| `nvme_tcp_specs.sc_name`     | Name of the NVMe/TCP StorageClass to delete.                         | `ontap-nvme-tcp-sc`                      |
| `fcp_specs.sc_name`          | Name of the FCP StorageClass to delete.                              | `ontap-fcp-sc`                           |

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
    nfs_specs:
      sc_name: ontap-nfs-sc
  roles:
    - delete_storageclass
```
