# set_default_storageclass role

## Overview

The `set_default_storageclass` role marks a single Kubernetes StorageClass as the cluster-wide default, clearing the `storageclass.kubernetes.io/is-default-class` annotation from any currently default StorageClass first.

## Features

- Fetches all existing StorageClasses in the cluster.
- Removes the default annotation from any existing default StorageClass.
- Sets the specified StorageClass as the new default.

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* The target StorageClass must already exist (typically created by the `create_storageclass` role).

## Role Variables

| Variable        | Description                                                                                              | Default                |
|-----------------|----------------------------------------------------------------------------------------------------------|------------------------|
| `oc_api_url`    | OpenShift/Kubernetes API URL.                                                                            | `https://api.example.openshift.com:6443` |
| `oc_api_token`  | OpenShift/Kubernetes API token.                                                                          | `<your_api_token>`     |
| `default_sc`    | Name of the StorageClass to mark as the cluster default. Must match one of the created StorageClasses.   | `ontap-nfs-sc`         |
| `configure_*`   | Backend enable flags (`configure_nfs`, `configure_nfs_flexgroup`, `configure_iscsi`, `configure_nvme_tcp`, `configure_fcp`). At least one must be `true` for this role to run. | **Required** (no default) |

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
    default_sc: ontap-nfs-sc
  roles:
    - set_default_storageclass
```
