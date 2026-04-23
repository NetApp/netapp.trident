# create_pvcs role

## Overview

The `create_pvcs` role is part of the **NetApp Trident Validated Content Collection**. It creates a Kubernetes/OpenShift namespace and provisions `PersistentVolumeClaim` (PVC) resources backed by **NetApp Trident** (`csi.trident.netapp.io`) StorageClasses for NFS, NFS FlexGroup, iSCSI, NVMe/TCP, and FCP backends.

## Features

- Creates the target namespace for PVCs if it does not already exist.
- Creates PVCs per backend using the corresponding `*_specs.pvc_info` list.
- Skips PVCs with unsupported combinations and prints a warning:
  - NFS FlexGroup PVCs smaller than `800Gi`.
  - iSCSI / NVMe/TCP / FCP PVCs using `ReadWriteMany` with `Filesystem` volume mode.

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and a valid API token.
* StorageClasses referenced by `sc_name` must already exist (typically created by the `create_storageclass` role).

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `create_pvcs_oc_api_url` | OpenShift/Kubernetes API server URL (for example, `https://api.cluster.example.com:6443`). | **Required** (no default) |
| `create_pvcs_oc_api_token` | OpenShift/Kubernetes bearer token used to authenticate against the API. | **Required** (no default) |
| `create_pvcs_validate_certs` | Whether to validate the TLS certificate of the Kubernetes API server. Set to `true` in production when a trusted CA is configured. | `false` |
| `create_pvcs_pvc_namespace` | Namespace in which to create the PVCs. Created by the role if it does not already exist. | **Required** (no default) |
| `create_pvcs_configure_nfs` | Set to `true` to create NFS PVCs from `create_pvcs_nfs_specs`. | `false` |
| `create_pvcs_configure_nfs_flexgroup` | Set to `true` to create NFS FlexGroup PVCs from `create_pvcs_nfs_flexgroup_specs`. | `false` |
| `create_pvcs_configure_iscsi` | Set to `true` to create iSCSI PVCs from `create_pvcs_iscsi_specs`. | `false` |
| `create_pvcs_configure_fcp` | Set to `true` to create FCP PVCs from `create_pvcs_fcp_specs`. | `false` |
| `create_pvcs_configure_nvme_tcp` | Set to `true` to create NVMe/TCP PVCs from `create_pvcs_nvme_tcp_specs`. | `false` |
| `create_pvcs_nfs_specs` | Dict with `sc_name` (StorageClass name) and `pvc_info` (list of PVCs) for NFS. | **Required** when `create_pvcs_configure_nfs` is `true` |
| `create_pvcs_nfs_flexgroup_specs` | Dict with `sc_name` and `pvc_info` for NFS FlexGroup PVCs. Each PVC must be at least `800Gi`. | **Required** when `create_pvcs_configure_nfs_flexgroup` is `true` |
| `create_pvcs_iscsi_specs` | Dict with `sc_name` and `pvc_info` for iSCSI PVCs. | **Required** when `create_pvcs_configure_iscsi` is `true` |
| `create_pvcs_nvme_tcp_specs` | Dict with `sc_name` and `pvc_info` for NVMe/TCP PVCs. | **Required** when `create_pvcs_configure_nvme_tcp` is `true` |
| `create_pvcs_fcp_specs` | Dict with `sc_name` and `pvc_info` for FCP PVCs. | **Required** when `create_pvcs_configure_fcp` is `true` |

Each entry in `pvc_info` is a dict with the following keys:

| Key | Description | Default |
|-----|-------------|---------|
| `pvc_name` | Name of the PersistentVolumeClaim to create. | **Required** (no default) |
| `pvc_size` | Requested PVC size (for example, `100Gi`). For NFS FlexGroup, must be `>= 800Gi`. | **Required** (no default) |
| `access_modes` | List of PVC access modes (for example, `[ReadWriteOnce]`, `[ReadWriteMany]`). | **Required** (no default) |
| `volume_mode` | PVC volume mode (`Filesystem` or `Block`). Applies to SAN backends (iSCSI, NVMe/TCP, FCP). | `Filesystem` |

> Note: At least one `create_pvcs_configure_*` flag must be `true`, and the corresponding `*_specs` variable must be set.

## Example Playbook

```yaml
---
- name: Create Trident-backed PVCs
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    create_pvcs_oc_api_url: "https://api.aa02-ocp.example.com:6443"
    create_pvcs_oc_api_token: "{{ OC_API_TOKEN }}"
    create_pvcs_pvc_namespace: test-project
    create_pvcs_configure_nfs: true
    create_pvcs_nfs_specs:
      sc_name: ontap-nfs-sc
      pvc_info:
        - { pvc_name: test-nfs-pvc-1, pvc_size: 100Gi, access_modes: [ReadWriteOnce] }
        - { pvc_name: test-nfs-pvc-2, pvc_size: 200Gi, access_modes: [ReadWriteMany] }
  roles:
    - create_pvcs
```
