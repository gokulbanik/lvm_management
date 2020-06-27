LVM Ansible role
================

Role to manage LVM Groups/Logical Volumes. Can be used to create, extend or resize LVM Groups and volumes.

Requirements
------------

Devices/disks to be part of the LVM setup must be identified prior to using this role. Ensure that you select the correct devices/disks.

Role Variables
--------------

`lvm_groups` is a list containing vgs.

### vgs

- `vgname`: uniq name
- `disks`: add disks/partitions to vg (comma separated)
- `create`: boolean (true => creates, false => deletes)
- `lvnames`: lv list (see bellow)

### lvnames

- `lvname`: uniq name
- `size`: define size of lvol (ex: "10G", "512M"...)
- `create`: defines if lvol should exist or be removed...true or false
- `filesystem`: defines filesystem to format lvol as
- `mount`: defines if filesystem should be mounted
- `mount_point`: defines mountpoint
- `mount_options`: defines mount options (comma separated)

Dependencies
------------

None

Example Playbook
----------------

- hosts: awx
  become: true
  vars_prompt:
    - name: "raw_disks"
      prompt: "Raw disk used for create a Physical Volume"
      private: no

    - name: "vg_name"
      prompt: "Name of the Volume Group"
      private: no

  vars:
    lvm_apply: true
    lvm_groups:
      - vgname: "{{ vg_name }}"
        disks: "{{ raw_disks }}"
        create: true
        lvnames:
          - lvname: lv.poy.was85
            size: 100m
            create: true
            filesystem: ext4
            mount: true
            mount_point: /poy/was85
            mount_options: 'defaults,noatime'
          - lvname: lv.poy.was85.was
            size: 10g
            create: true
            filesystem: ext4
            mount: true
            mount_point: /poy/was85/was
            mount_options: 'defaults,noatime'
      # VG whitout LV
      - vgname: "{{ vg_name }}"
        disks: "{{ raw_disks }}"
        create: true
        lvnames: []

  roles:
     - lvm_management


License
-------

BSD



Author Information
------------------

-
