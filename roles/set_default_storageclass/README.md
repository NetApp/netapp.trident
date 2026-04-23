# Set Default StorageClass Role

## Overview

The `set_default_storageclass` role is part of the **NetApp Trident Validated Content Collection**. It marks a single Kubernetes/OpenShift `StorageClass` as the cluster-wide default, first clearing the `storageclass.kubernetes.io/is-default-class` annotation from any StorageClass that is currently marked as default.

## Features

- Fetches all existing StorageClasses in the cluster.
- Removes the default-class annotation from any currently default StorageClass.
- Sets the specified StorageClass as the new cluster-wide default.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.
* The target StorageClass must already exist (typically created by the `create_storageclass` role).

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `set_default_storageclass_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `set_default_storageclass_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `set_default_storageclass_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `set_default_storageclass_default_sc` | Name of the StorageClass to mark as the cluster-wide default. Must match an existing StorageClass. | **Required** (no default) |
| `set_default_storageclass_configure_nfs` | Set to `true` if the NFS StorageClass is in scope for the default-class selection. | `false` |
| `set_default_storageclass_configure_nfs_flexgroup` | Set to `true` if the NFS FlexGroup StorageClass is in scope for the default-class selection. | `false` |
| `set_default_storageclass_configure_iscsi` | Set to `true` if the iSCSI StorageClass is in scope for the default-class selection. | `false` |
| `set_default_storageclass_configure_fcp` | Set to `true` if the FCP StorageClass is in scope for the default-class selection. | `false` |
| `set_default_storageclass_configure_nvme_tcp` | Set to `true` if the NVMe/TCP StorageClass is in scope for the default-class selection. | `false` |

> Note: At least one `set_default_storageclass_configure_*` flag must be `true` for this role to run.

## Example Playbook

```yaml
---
- name: Set the NFS StorageClass as the default storage class
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    set_default_storageclass_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    set_default_storageclass_oc_api_token: "{{ OC_API_TOKEN }}"
    set_default_storageclass_configure_nfs: true
    set_default_storageclass_default_sc: ontap-nfs-sc
  roles:
    - set_default_storageclass
```
