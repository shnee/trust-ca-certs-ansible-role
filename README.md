Ansible Role: trust-ca-certs
================================================================================

An ansible role to add a certificate to the system's cert trust.

Currently this role will only work and Debian based distro's and has only been
tested on Ubuntu 20.04.

Role Variables
----------------------------------------

```yaml
# These packages are required for this role to work. The second layer of the
# dict must match `ansible_os_family` of the remote host.
dep_packages:
  Debian: [ca-certificates]
  RedHat: []

# This is a list of certs in the format:
# - name: <name of cert>
#   base_64_content: <base64 encoded content of the cert file>
#
# The content must be in pem format and the file name must end in .crt for
# Ubuntu.
certs: []

# The location that certs must be put before they get added to the trust. The
# second layer of the dict must match `ansible_os_family` of the remote host.
remote_cert_location:
  Debian: /usr/local/share/ca-certificates

# The location where certs are put after being processed. The second layer of
# the dict must match `ansible_os_family` of the remote host.
remote_store_cert_location:
  Debian: /etc/ssl/certs
```

Example Playbook
----------------------------------------

```yaml
- name: Add certificate to system's trust.
  hosts: all
  vars:
    certs:
      - name: my-root-ca.crt
        base_64_content: "{{ my_root_ca_cert_pem_base_64 }}"
      - name: other-root-ca.crt
        base_64_content: "{{ other_root_ca_crt_base_64 }}"
  vars_files:
    - ./certs.yml
  roles:
    - trust-ca-certs
```

License
----------------------------------------

MIT

Author Information
----------------------------------------

This role was created by [shnee](https://github.com/shnee).
