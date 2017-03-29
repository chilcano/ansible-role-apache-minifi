# Ansible Role: apache-minifi

An Ansible Role that installs, configures and runs [Apache MiNiFi](https://nifi.apache.org/minifi) in tiny devices like a Raspberry Pi, although you can use it on any distro. This Role provides the following features:

- Install Apache MiNiFi and Java SDK.
- Configure Apache MiNiFi.
- Run Apache MiNiFi as a `systemd` service.

## Requirements

- A tiny device like Raspberry Pi with minimalist distro based on Debian, Ubuntu, CentOS, etc.
- This device connected to your LAN with assigned IP address.

## Role Variables

Default variables is in `defaults/main.yml`.

### Actions to be executed
```
minifi_action_clean: true
minifi_action_install: true
minifi_action_run: true
```

### Variables to download and install
```
localmirror_dir_name_www: "localmirror"
#minifi_url_bin_installer: "https://public-repo-1.hortonworks.com/HDF/2.1.2.0/minifi-1.0.2.1.2.0-10-bin.tar.gz"
#minifi_url_bin_installer: "http://apache.mirror.anlx.net/nifi/minifi/0.1.0/minifi-0.1.0-bin.tar.gz"
minifi_url_bin_installer: "http://{{ groups['pibuilder'][0] }}/{{ localmirror_dir_name_www }}/minifi-{{ minifi_version }}-bin.{{ minifi_packaging }}"
minifi_version: "0.1.0"
minifi_packaging: "tar.gz"
```

### Variables to clean
```
minifi_remove_dependencies: false
```

### Variables to run
```
minifi_service_name: minifipi
```

## Dependencies

None.

## Example Playbook

Using the Ansible Role for installing, configuring and running Apache MiNiFi. The idea is to connect the preinstalled Kismet with Apache MiNiFi, and forward the captured traffic to Apache NiFi running in other VM or Server.

```
---
- hosts: piminifis
  become: yes

  vars_files:
    - vars.yml

  roles:
    - { role: chilcano.apache-minifi }
```

The `inventory` file contains:
```
[pibuilder]
192.168.1.204

[pikismets]
192.168.1.35

[piminifis]
192.168.1.35

[nifis]
192.168.1.100
```


## License

MIT / BSD

## Author Information

This role was created in 2017 by [Roger Carhuatocto](https://www.linkedin.com/in/rcarhuatocto), author of [HolisticSecurity.io Blog](https://holisticsecurity.io).
