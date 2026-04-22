# Delete PVCs Role

## Overview

The `delete_pvcs` role deletes `PersistentVolumeClaim` resources from a namespace and then deletes the namespace itself.

## Features

- Deletes PVCs per backend from `pvc_namespace` based on the `pvc_info` lists in each `*_specs` variable.
- Deletes `pvc_namespace` after all per-backend PVC deletions.

## Requirements

* Ansible v2.16.0 or newer
* Python 3.12 or newer
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.

## Role Variables

| Variable                       | Description                                                           | Default        |
|--------------------------------|-----------------------------------------------------------------------|----------------|
| `oc_api_url`                   | OpenShift/Kubernetes API URL.                                         |                |
| `oc_api_token`                 | OpenShift/Kubernetes API token.                                       |                |
| `pvc_namespace`                | Namespace whose PVCs will be removed and that will itself be deleted. | `test-project` |
| `configure_nfs`                | Set to `true` to delete NFS PVCs.                                     | `false`        |
| `configure_nfs_flexgroup`      | Set to `true` to delete NFS FlexGroup PVCs.                           | `false`        |
| `configure_iscsi`              | Set to `true` to delete iSCSI PVCs.                                   | `false`        |
| `configure_fcp`                | Set to `true` to delete FCP PVCs.                                     | `false`        |
| `configure_nvme_tcp`           | Set to `true` to delete NVMe/TCP PVCs.                                | `false`        |
| `nfs_specs.pvc_info`           | List of NFS PVC entries (with `pvc_name`) to delete.                  | See defaults   |
| `nfs_flexgroup_specs.pvc_info` | List of NFS FlexGroup PVC entries to delete.                          | See defaults   |
| `iscsi_specs.pvc_info`         | List of iSCSI PVC entries to delete.                                  | See defaults   |
| `nvme_tcp_specs.pvc_info`      | List of NVMe/TCP PVC entries to delete.                               | See defaults   |
| `fcp_specs.pvc_info`           | List of FCP PVC entries to delete.                                    | See defaults   |

## Example Playbook

```yaml
---
- name: Delete Trident PVCs
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    pvc_namespace: test-project
    configure_nfs: true
    nfs_specs:
      pvc_info:
        - { pvc_name: test-nfs-pvc-1 }
  roles:
    - delete_pvcs
```
