# delete_pvcs role

## Overview

The `delete_pvcs` role is part of the **NetApp Trident Validated Content Collection**. It deletes `PersistentVolumeClaim` resources from a namespace for the enabled backends, and then deletes the namespace itself.

## Features

- Deletes PVCs per backend from `delete_pvcs_pvc_namespace` based on the `pvc_info` list inside each `*_specs` variable.
- Deletes the `delete_pvcs_pvc_namespace` namespace after all per-backend PVC deletions complete.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `delete_pvcs_oc_api_url` | OpenShift/Kubernetes API server URL. | **Required** (no default) |
| `delete_pvcs_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `delete_pvcs_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `delete_pvcs_pvc_namespace` | Namespace whose PVCs will be removed; the namespace itself is also deleted at the end of the run. | **Required** (no default) |
| `delete_pvcs_configure_nfs` | Set to `true` to delete the NFS PVCs listed in `delete_pvcs_nfs_specs`. | `false` |
| `delete_pvcs_configure_nfs_flexgroup` | Set to `true` to delete the NFS FlexGroup PVCs listed in `delete_pvcs_nfs_flexgroup_specs`. | `false` |
| `delete_pvcs_configure_iscsi` | Set to `true` to delete the iSCSI PVCs listed in `delete_pvcs_iscsi_specs`. | `false` |
| `delete_pvcs_configure_fcp` | Set to `true` to delete the FCP PVCs listed in `delete_pvcs_fcp_specs`. | `false` |
| `delete_pvcs_configure_nvme_tcp` | Set to `true` to delete the NVMe/TCP PVCs listed in `delete_pvcs_nvme_tcp_specs`. | `false` |
| `delete_pvcs_nfs_specs` | Dict with `pvc_info` list of NFS PVCs to delete. | **Required** when `delete_pvcs_configure_nfs` is `true` |
| `delete_pvcs_nfs_flexgroup_specs` | Dict with `pvc_info` list of NFS FlexGroup PVCs to delete. | **Required** when `delete_pvcs_configure_nfs_flexgroup` is `true` |
| `delete_pvcs_iscsi_specs` | Dict with `pvc_info` list of iSCSI PVCs to delete. | **Required** when `delete_pvcs_configure_iscsi` is `true` |
| `delete_pvcs_nvme_tcp_specs` | Dict with `pvc_info` list of NVMe/TCP PVCs to delete. | **Required** when `delete_pvcs_configure_nvme_tcp` is `true` |
| `delete_pvcs_fcp_specs` | Dict with `pvc_info` list of FCP PVCs to delete. | **Required** when `delete_pvcs_configure_fcp` is `true` |

Each entry in `pvc_info` is a dict with:

| Key | Description | Default |
|-----|-------------|---------|
| `pvc_name` | Name of the PVC to delete from `delete_pvcs_pvc_namespace`. | **Required** (no default) |

> Note: At least one `delete_pvcs_configure_*` flag must be `true`.

## Example Playbook

```yaml
---
- name: Delete Trident PVCs
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    delete_pvcs_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    delete_pvcs_oc_api_token: "{{ OC_API_TOKEN }}"
    delete_pvcs_pvc_namespace: test-project
    delete_pvcs_configure_nfs: true
    delete_pvcs_nfs_specs:
      pvc_info:
        - { pvc_name: test-nfs-pvc-1 }
        - { pvc_name: test-nfs-pvc-2 }
  roles:
    - delete_pvcs
```
