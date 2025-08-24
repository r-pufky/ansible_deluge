# Deluge
Deluge bittorrent client.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_deluge/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_deluge/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_deluge/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## Example Playbook
Read defaults documentation. Integrated and external plugins are supported.

site.yml
``` yaml
- name: 'Deluge server'
  hosts: 'deluge.example.com'
  become: true
  roles:
     - 'r_pufky.srv.deluge'
  vars:
    deluge_cfg_web_pwd_salt: '{{ vault_deluge_salt }}'
    deluge_cfg_web_pwd_sha1: '{{ vault_deluge_sha1 }}'
    deluge_cfg_core_enabled_plugins:
      - 'AutoAdd'
      - 'Blocklist'
      - 'Extractor'
      - 'Label'
      - 'Scheduler'
      - 'Toggle'
    deluge_cfg_plugins_auto_add:
      - id: 1
        abs_path: '/data/downloads'
        enabled: true
    deluge_cfg_plugins_blocklist:
      url: 'http://example.com/blocklist'
      load_on_start: true
      whitelisted:
        - '172.31.234.32'
    deluge_cfg_plugins_extractor:
      extract_path: '/tmp'
      use_name_folder: true
    deluge_cfg_plugins_label:
      - name: 'isos'
        apply_max: true
        auto_add: true
        auto_add_trackers:
          - 'os'
        prioritize_first_last: true
    deluge_cfg_plugins_scheduler:
      mon: [1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 0, 0, 0, 0, 0, 0, 0, 0]
      low_active: -1
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Major release versions track Debian release versions:

* **[13.x.x](https://github.com/r-pufky/ansible_deluge)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_deluge/tree/12.x)**: 12 Bookworm.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_deluge/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
