========================================================
Release Notes: Ansible Validated Content for NetApp Trident
========================================================

.. contents:: Topics

v1.0.0
======

Release Summary
---------------

This is the first major release of the ``netapp.trident`` collection.

New Roles
---------

- netapp.trident.create_storageclass - Create Kubernetes StorageClasses backed by NetApp Trident (NFS, NFS FlexGroup, iSCSI, NVMe/TCP, FCP)
- netapp.trident.set_default_storageclass - Set a Kubernetes StorageClass as the cluster-wide default
- netapp.trident.create_pvcs - Create a namespace and PersistentVolumeClaims using Trident StorageClasses
- netapp.trident.create_volume_snapshotclass - Create a Trident-driven VolumeSnapshotClass
- netapp.trident.create_volume_snapshots - Create VolumeSnapshots of existing PVCs
- netapp.trident.delete_volume_snapshots - Delete VolumeSnapshots from a namespace
- netapp.trident.delete_volume_snapshotclass - Delete a VolumeSnapshotClass
- netapp.trident.delete_pvcs - Delete PVCs and the namespace used for provisioning
- netapp.trident.delete_storageclass - Delete Trident-backed StorageClasses
