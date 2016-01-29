Test playbook for AnsibleSpec variable support
================================================

This repository is the test playbook for Ansible 
Variable support is requested in
[PR#60](https://github.com/volanja/ansible_spec/pull/60).

This supports overwritten variables. 
See http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable.

Only following features are supported currently.

 * role vars (roles/xxx/vars/main.yml)
 * play vars (in site.yml and included playbook)
 * host vars
 * group vars

Instruction
-----------

Config network:

```
  # ip addr add 192.168.1.1/24 dev eth0
  # ip addr add 192.168.1.2/24 dev eth0
```

Run Playbook:

```
  # ansible site.yml -i hosts -l group1
  ..
  TASK [test : debug] *********
  ok: [192.168.1.1] => {
      "msg": "role_var=role"
  }
  
  TASK [test : debug] *********
  ok: [192.168.1.1] => {
      "msg": "nested_role_var=nested"
  }
  ..
```

Run Serverspec:

```
  # rake serverspec:group1
  ...
  role_var:role
  nested_role_var:nested
  site_var:site
  host_var:host 192.168.1.1
  group_var:group all
  group_all_var:group all
```

Then compare Ansible and Serverspec output values.
You can also test for group2 and all groups. 




