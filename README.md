# Deluge
Deluge bittorrent client.

## [Requirements][i]
Requires [r_pufky.arr][g] galaxy-ng collection. See
[reference documentation][h] for troubleshooting and config variables.

Install size: ~13MB

Use absolute paths for directories in core.conf. Default is
**/var/opt/deluge**.

> Tasks [potentially touching Network Mounted Filesystems][o] will be run as
> the task user and fallback to the service user. Manage these locations
> externally if these fail.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
> NOTE:
> Deluge will silently fail parsing configuration files and continue
> starting - leading to a partially configured state.
>
> May be expressed as 'working' but not all set options configured or work;
> these will all occur after the parsing error. This applies to all configs.
>
> Debugging requires working backwards in the config until the errors are
> found.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                   | Notes
 ------|------------------------|-------
  1    | deluge_flg_apt_sources | Force required APT sources.
  2    | deluge_flg_install     | Install deluge.
  3    | deluge_flg_config      | Deploy configuration files.
  4    | deluge_flg_perms       | Set permissions based on core.conf.

### Example Playbooks

#### New Deployments
[A minimal config][m] will be deployed if no configuration is specified. This
will deploy a working Deluge install.

``` yaml
# Main directory: /var/opt/deluge.
- name: 'Deluge default install.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.deluge'
```

#### Existing Deployments
Configuration directory deployed as templates, allowing for vault use of
sensitive information. File permissions can be set based on the configured
directories in **core.conf** to ensure the service can manage the files. Files
not present in the source config directory will be removed on the remote.

``` yaml
- name: 'Set custom config, and ensure file permissions set.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.deluge'
  vars:
    deluge_flg_config: true
    deluge_flg_perms: true
    deluge_cfg_d: 'host_vars/deluge.example.com/config'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

 Release | Debian | Ansible | Notes
---------|--------|---------|-------
 5.x.x   | 13     | 2.20    | Ansible 2.20, feature flags, and semantic versioning.
 4.x.x   | 12     | 2.18    | Migrate to r_pufky.arr.
 3.x.x   | 13     | 2.18    | Migrate to Debian Trixie.
 2.x.x   | 12     | 2.18    | Use libraries for common operations.
 1.x.x   | 12     | 2.11    | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_deluge/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_arr
[h]: https://r-pufky.github.io/docs/arr/deluge
[i]: https://github.com/r-pufky/ansible_deluge/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_deluge/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_deluge/blob/main/defaults/main/ports.yml
[m]: https://github.com/r-pufky/ansible_deluge/blob/main/templates/default/core.conf
[o]: https://r-pufky.github.io/ansible_docs/best_practice/patterns/#network-mounts