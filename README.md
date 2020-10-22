[![](https://img.shields.io/travis/com/ckaserer/ansible-role-authorized-keys/master?style=flat-square)](https://travis-ci.com/ckaserer/ansible-role-authorized-keys)
![gplv3](https://img.shields.io/badge/license-GPL%20v3.0-brightgreen.svg?style=flat-square)
![Maintenance](https://img.shields.io/maintenance/yes/2021?style=flat-square)

# ckaserer.authorized_keys

First off, we need to install the latest version of the ckaserer.authorized_keys ansible role from ansible galaxy via

```
ansible-galaxy install ckaserer.authorized_keys
```

---

## Set authorized_keys via ansible

The playbook below adds `my-ssh-key` to the `authorized_keys` file for the user `ckaserer` on all target hosts allowing remote ssh access to the specified hosts using `my-ssh-key` for the user `ckaserer`.

Alternativly you can set `hosts` to a group of ansible nodes or `localhost`.

```
- hosts: all
  tasks:
    - name: Include ckaserer.authorized_keys
      include_role:
        name: ckaserer.authorized_keys
      vars:
        authorization_list:
          - name: ckaserer
            ssh_keys:
              - my-ssh-key
        select_public_keys_from_path: ./public-keys
```

Below you have the folder structure required by the example playbook above. In the playbook we set `select_public_keys_from_path` to `./public-keys`. Hence the public key `my-ssh-key` specified in the `authorization_list` for the user `ckaserer` is located in the folder `public-keys` on the same hierarchy level as the playbook.

```
├── playbook.yml
└── public-keys
    └── my-ssh-key.pub
```

----

## Available variables and defaults

The available parameters can be used to alter the behaviour of the role to fit your specific requirements.

| Variable  | Default | Notes |
| ------------- | ------------- | ------------- |
| authorization_list | [] | Specify the users that shall be managed by the ansible role and the associated keys |
| create_backup | true | `true`: Create a backup of authorized_keys before updating authorized_keys |
| exclusive_authorized_keys | false | `true`: only keys specified in `authorization_list` will be kept in the authorized_keys file |
| fail_when_user_not_found | true | The ansible role will fail when a user specified in `authorization_list` does not exist on the target node |
| restore_authorized_keys | false | In you need to role back to a previsously backed up version of authorized_keys you can set `restore_authorized_keys` to `true`. If `restore_authorized_keys_from_file` is not set it will roll back to the latest backup |
| restore_authorized_keys_from_file | not defined | When rolling back to a previous backup by setting `restore_authorized_keys` to `true` you can specify the backup to roll back to by setting `restore_authorized_keys_from_file` to a specific backup. e.g. `restore_authorized_keys_from_file: ~/.ssh/authorized_keys.bak` |
| select_public_keys_from_path | ./public-keys | Specify where to look for public keys specified by `authorization_list` |
