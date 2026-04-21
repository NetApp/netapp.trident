[![CI](https://github.com/NetApp/netapp.trident/workflows/CI/badge.svg?event=push)](https://github.com/NetApp/netapp.trident/actions)

# Ansible Validated Content Collection for NetApp Trident

This collection includes a variety of validated Ansible roles to automate and
manage Day-2 use cases involving NetApp Trident in an OpenShift Virtualization environment.

## Dependencies

This collection uses the [kubernetes.core](https://galaxy.ansible.com/ui/repo/published/kubernetes/core)
certified Ansible collection to manage Kubernetes objects such as StorageClasses,
PersistentVolumeClaims (PVCs), and VolumeSnapshots via the NetApp Trident CSI driver.

## Tested with Ansible

This collection has been tested with ansible-core >= 2.16.0.

## Using this collection

### Installing the Collection from Ansible Galaxy

Before using this collection, you need to install it with the Ansible Galaxy
command-line tool:

```bash
ansible-galaxy collection install netapp.trident
```

You can also include it in a `requirements.yml` file and install it with
`ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: netapp.trident
```

To upgrade the collection to the latest available version, run:

```bash
ansible-galaxy collection install netapp.trident --upgrade
```

To install a specific version, for example `1.0.0`:

```bash
ansible-galaxy collection install netapp.trident:==1.0.0
```

See [using Ansible collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) for more details.

### Running the playbook

To configure Kubernetes objects using NetApp Trident:

```bash
ansible-playbook Setup_Trident_tasks.yml -e @vars/trident_main.yml -t trident_setup_tasks
```

To delete the created Kubernetes objects:

```bash
ansible-playbook Setup_Trident_tasks.yml -e @vars/trident_main.yml -t trident_cleanup_tasks
```

Update `vars/trident_main.yml` with your desired configuration before running the playbook.

## Release notes

See the [changelog](https://github.com/NetApp/netapp.trident/tree/main/changelogs/changelog.rst).

## More information

* [NetApp Trident documentation](https://docs.netapp.com/us-en/trident/)
* [kubernetes.core Ansible collection](https://galaxy.ansible.com/ui/repo/published/kubernetes/core)
* [Ansible user guide](https://docs.ansible.com/ansible/devel/user_guide/index.html)
* [Ansible developer guide](https://docs.ansible.com/ansible/devel/dev_guide/index.html)
* [Ansible collections requirements](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html)
* [Ansible community Code of Conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html)
* [The Bullhorn (the Ansible contributor newsletter)](https://docs.ansible.com/ansible/devel/community/communication.html#the-bullhorn)
* [Important announcements for maintainers](https://github.com/ansible-collections/news-for-maintainers)
