# IMS - Ansible Playbooks

This repository contains the playbooks used in pre/post-migratio with
Infrastructure Migration Solution (IMS). The playbooks mainly wrap calls to
roles that are maintained in separated repositories, so very little development
is done in this repository.

## Pre-migration - Red Hat Enterprise Linux

This playbook does 2 things:

1. Create a udev rules file that associate the names of the NICs to their MAC addresses.
  This allows to keep a consistent naming across virtualization platform, and to keep static networking working on Linux.

2. Install `qemu-guest-agent` package. This allows RHV to know the IP address of the virtual machine. In turn, CloudForms can collect it during the inventory and we can use it in post-migration playbook inventory.

The playbook depends on a role named [fdupont_redhat.ims_premigration_rhel](https://galaxy.ansible.com/fdupont_redhat/ims_premigration_rhel)

## Post-migration - Convert2RHEL

This playbook converts the virtual machine operating system from CentOS or Oracle Linux to Red Hat Enterprise Linux.
During a migration, we can add it as post-migration playbook and the virtual machine will be converted.
The playbook requires some variables to be set:

- `rhsm_username` - The username to register the virtual machine to RHSM. Red Hat Satellite 6 is not supported.
- `rhsm_password` - The password to register the virtual machine to RHSM. Activation keys are not supported.
- `rhsm_pool` - The subscription pool to attach to the virtual machine.
- `rhel_variant` - The variant of RHEL. Only `Server` is supported.
- `convert2rhel_package_url` - A URL where `convert2rhel` package can be downloaded. It's not part of any official repository.

During the conversion, the operating system is protected by a snapshot. It is created as soon as the playbook starts and
can be used to rollback.

## Pre-migration - Windows

This playbook configures the Windows virtual machine for migration.
The only variables that you may want to customize are the ones configuring
the Ansible WinRM connection.See
[Windows Remote Management > Inventory Options](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#inventory-options)


## Post-migration - Leapp

Work in progress.
