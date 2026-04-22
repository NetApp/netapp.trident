# create_pvcs role

## Overview

The `create_pvcs` role creates a Kubernetes namespace and provisions `PersistentVolumeClaim` (PVC) resources backed by Trident StorageClasses for NFS, NFS FlexGroup, iSCSI, NVMe/TCP, and FCP backends.

## Features

- Creates the target namespace for PVCs if it does not already exist.
- Creates PVCs per backend using the corresponding `*_specs.pvc_info` list.
- Skips PVCs with unsupported combinations and prints a warning:
  - NFS FlexGroup PVCs smaller than 800Gi.
  - iSCSI / NVMe/TCP / FCP PVCs with `ReadWriteMany` + `Filesystem` volume mode.

## Requirements

* Ansible 2.15 or newer.
* The [Kubernetes.Core](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/index.html#plugins-in-kubernetes-core) Ansible collection is installed.
* OpenShift/Kubernetes cluster reachable via API URL and valid API token.
* StorageClasses referenced by `sc_name` must already exist (typically created by the `create_storageclass` role).

## Role Variables

| Variable                | Description                                                                                           | Default                                  |
|-------------------------|-------------------------------------------------------------------------------------------------------|------------------------------------------|
| `oc_api_url`            | OpenShift/Kubernetes API URL.                                                                         | `https://api.example.openshift.com:6443` |
| `oc_api_token`          | OpenShift/Kubernetes API token.                                                                       | `<your_api_token>`                       |
| `pvc_namespace`         | Namespace where the PVCs will be created.                                                             | `test-project`                           |
| `configure_*`           | Backend enable flags (`configure_nfs`, `configure_nfs_flexgroup`, `configure_iscsi`, `configure_nvme_tcp`, `configure_fcp`). | `true` |
| `nfs_specs`             | `sc_name` + `pvc_info` list for NFS PVCs.                                                             | See defaults                             |
| `nfs_flexgroup_specs`   | `sc_name` + `pvc_info` list for NFS FlexGroup PVCs (minimum size 800Gi).                              | See defaults                             |
| `iscsi_specs`           | `sc_name` + `pvc_info` list for iSCSI PVCs.                                                           | See defaults                             |
| `nvme_tcp_specs`        | `sc_name` + `pvc_info` list for NVMe/TCP PVCs.                                                        | See defaults                             |
| `fcp_specs`             | `sc_name` + `pvc_info` list for FCP PVCs.                                                             | See defaults                             |

Each entry in `pvc_info` accepts `pvc_name`, `pvc_size`, `access_modes`, and (for SAN backends) `volume_mode`.

## Example Playbook

```yaml
---
- name: Create Trident-backed PVCs
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    pvc_namespace: test-project
    nfs_specs:
      sc_name: ontap-nfs-sc
      pvc_info:
        - { pvc_name: test-nfs-pvc-1, pvc_size: 100Gi, access_modes: [ReadWriteOnce] }
  roles:
    - create_pvcs
```
