ocp-3.11-containers
=========

An Ansible Role to pull OpenShift 3.11 related containers to deploy in disconnected environments.

Requirements
------------

- Ansible >= 2.4
- RHEL == 7
- Active RHEL Subscription
- Recommended to have a system with 150 GB of space

Role Variables
--------------

| Parameter | Type | Required | Default Value | Comments |
| --- | --- | --- | --- | --- |
| **additional_containers** | list | no | *undefined* | List of additional containers to pull into the content_sync |
| **containers_pull** | list | yes | *see role defaults* | List of containers required to run OCP 3.11, see [disconnected_install](https://docs.openshift.com/container-platform/3.11/install/disconnected_install.html) |
| **content_sync_path** | string | yes | `/data` | The destination path for the openscap OVAL definitions |
| **registry_authfile_path** | string | no | *undefined* | Credential authfile path used when authenticating to https://registry.redhat.io |
| **registry_authtype** | string | yes | *credential* | Credential type used when authenticating to https://registry.redhat.io, values: [credential, config] |
| **registry_username** | string | yes | *undefined* | Username to use when authenticating to https://registry.redhat.io |
| **registry_password** | string | yes | *undefined* | Password to use when authenticating to https://registry.redhat.io |

### If Running on a RHEL System these variables handle connecting to RHSM to install Podman
| Parameter | Type | Required | Default Value | Comments |
| --- | --- | --- | --- | --- |
| **rhsm_user** | string | no | *undefined* | The account username with access to subscriptions on https://access.redhat.com, **required only when running from a RHEL system** |
| **rhsm_password** | string | no | *undefined* | The account password with access to subscriptions on https://access.redhat.com, **required only when running from a RHEL system** |
| **rhsm_pool** | string | no | *undefined* | The subscription pool IDs to consume that contain OCP 3.11. (ex. `0123456789abcdef0123456789abcdef`), **only when running from a RHEL system** |

Role Tags
---------
| Tag | Comments |
| --- | --- |
| **cleanup** | Tag to run as last step to cleanup synced data stored in the `{{ temp_sync_path }}/containers` |

Example Playbook
----------------

### Running with defaults
```yaml
- name: ocp-3.11-containers example
  roles:
    - role: ocp-3.11-containers
      tags:
        - containers-pull
      vars:
        registry_password: aPassword
        registry_username: auser@example.com
```

### Running with registry service account authfile
```yaml
- name: ocp-3.11-containers example
  roles:
    - role: ocp-3.11-containers
      tags:
        - containers-pull
      vars:
        registry_authfile_path: path/to/authfile
        registry_authtype: config
```

### Running with additional containers
```yaml
- name: ocp-3.11-containers example
  roles:
    - role: ocp-3.11-containers
      tags:
        - containers-pull
      vars:
        additional_containers:
          - registry.redhat.io/rhel8
        registry_password: aPassword
        registry_username: auser@example.com
```

License
-------

GPLv3
