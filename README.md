# Ansible role [ruby_gems](https://galaxy.ansible.com/ui/standalone/roles/buluma/ruby_gems/documentation)

Ruby Gems installation for Linux.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-ruby_gems/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ruby_gems/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ruby_gems.svg)](https://github.com/buluma/ansible-role-ruby_gems/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-ruby_gems.svg)](https://github.com/buluma/ansible-role-ruby_gems/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-ruby_gems.svg)](https://github.com/buluma/ansible-role-ruby_gems/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/ruby_gems)](https://galaxy.ansible.com/ui/standalone/roles/buluma/ruby_gems/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-ruby_gems/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true

  vars:
    ruby_install_gems_user: root
    ruby_install_gems:
      - json
    ruby_gems_bin_path: /root/.gem/ruby/bin

  pre_tasks:
    - name: Update apt cache.
      apt: update_cache=true cache_valid_time=600
      when: ansible_os_family == 'Debian'

    - name: Add rubygems bin dir to system-wide $PATH.
      ansible.builtin.copy:
        dest: /etc/profile.d/ruby.sh
        content: 'PATH=$PATH:{{ ruby_gems_bin_path }}'
        mode: 0644

    - name: Don't install Bundler on CentOS 7 because of old Ruby version.
      ansible.builtin.set_fact:
        ruby_install_bundler: false
      when:
        - ansible_os_family == 'RedHat'
        - ansible_distribution_major_version == '7'

  roles:
    - role: buluma.bootstrap
    - role: buluma.ruby_gems

  post_tasks:
    - name: Verify Ruby is installed.
      command: ruby --version
      changed_when: false
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-ruby_gems/blob/master/defaults/main.yml):

```yaml
---
workspace: /root

# Whether this role should install Bundler.
ruby_install_bundler: true

# A list of Ruby gems to install.
ruby_install_gems: []

# The user account under which Ruby gems will be installed.
ruby_install_gems_user: "{{ ansible_user }}"

# If set to True, ruby will be installed from source, using the version set with
# the 'ruby_version' variable instead of using a package.
ruby_install_from_source: true
# TODO: Testing True
ruby_download_url: http://cache.ruby-lang.org/pub/ruby/3.0/ruby-3.0.0.tar.gz
ruby_version: "3.0.0"
ruby_source_configure_command: ./configure --enable-shared

# Default should usually work, but this will be overridden on Ubuntu 14.04.
ruby_rubygems_package_name: rubygems
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-ruby_gems/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-ruby_gems/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|bionic, focal, jammy|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|

The minimum version of Ansible required is 2.4, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-ruby_gems/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-ruby_gems/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-ruby_gems/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)

