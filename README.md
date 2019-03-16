loop
====

Execute a role for each set of provided parameters.

Requirements
------------

None.

Role Variables
--------------

See `defaults/main.yml`


```yaml
# role to loop over
loop_role: <your role here>  # required
# string to add as a prefix for role vars
loop_prefix: "{{ loop_role | lower | regex_replace('[-/]', '_') ~ '_' }}"
# items to loop over
loop_items: []  # required
```


Dependencies
------------

None.

Example Playbook
----------------

Example of calling the same role multiple times without loop role:

    - name: install a libvirt and openstack VM
      hosts: localhost
      roles:
      - role: vm
        vm_type: libvirt
        vm_host: virthost
        vm_cpu_cores: 4
        vm_memory: 4096
      - role: vm
        vm_type: openstack
        vm_memory: 4096
        vm_image: cirros

Example of calling the same role multiple times with loop role:

    - name: install a libvirt and openstack VM
      hosts: localhost
      roles:
      - role: loop
        loop_role: vm
        loop_items:
        - type: libvirt
          host: virthost
          cpu_cores: 4
          memory: 4096
        - type: openstack
          memory: 4096
          image: cirros

Same thing except passing list of items as a variable:

    - name: install a libvirt and openstack VM
      hosts: localhost
      vars:
        vm_list:
        - type: libvirt
          host: virthost
          cpu_cores: 4
          memory: 4096
        - type: openstack
          memory: 4096
          image: cirros
      roles:
      - role: loop
        loop_role: vm
        loop_items: "{{ vm_list }}"


License
-------

Apache-2.0 or MIT or BSD-3-Clause

Author Information
------------------

[James Cassell](https://github.com/jamescassell)
