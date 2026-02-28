# [Ansible role ruby_gems](#ansible-role-ruby_gems)

Ruby Gems installation for Linux.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-ruby_gems/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-ruby_gems/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-ruby_gems/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-ruby_gems)|[![downloads](https://img.shields.io/ansible/role/d/buluma/ruby_gems)](https://galaxy.ansible.com/buluma/ruby_gems)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ruby_gems.svg)](https://github.com/buluma/ansible-role-ruby_gems/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-ruby_gems/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- become: true
  hosts: all
  name: Converge
  post_tasks:
  - changed_when: false
    command: ruby --version
    name: Verify Ruby is installed.
  pre_tasks:
  - apt: update_cache=true cache_valid_time=600
    name: Update apt cache.
    when: ansible_os_family == 'Debian'
  - ansible.builtin.copy:
      content: PATH=$PATH:{{ ruby_gems_bin_path }}
      dest: /etc/profile.d/ruby.sh
      mode: 420
    name: Add rubygems bin dir to system-wide $PATH.
  - ansible.builtin.set_fact:
      ruby_install_bundler: false
    name: Don't install Bundler on CentOS 7 because of old Ruby version.
    when:
    - ansible_os_family == 'RedHat'
    - ansible_distribution_major_version == '7'
  roles:
  - role: buluma.bootstrap
  - role: buluma.ruby_gems
  vars:
    ruby_gems_bin_path: /root/.gem/ruby/bin
    ruby_install_gems:
    - json
    ruby_install_gems_user: root
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-ruby_gems/blob/master/defaults/main.yml):

```yaml
---
ruby_download_url: http://cache.ruby-lang.org/pub/ruby/3.0/ruby-3.0.0.tar.gz
ruby_install_bundler: true
ruby_install_from_source: true
ruby_install_gems: []
ruby_install_gems_user: '{{ ansible_user }}'
ruby_rubygems_package_name: rubygems
ruby_source_configure_command: ./configure --enable-shared
ruby_version: 3.0.0
workspace: /root
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-ruby_gems/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-ruby_gems/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|

The minimum version of Ansible required is 2.4, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-ruby_gems/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-ruby_gems/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)
