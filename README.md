[![Build Status](https://travis-ci.org/juju4/ansible-macos-apps-install.svg?branch=master)](https://travis-ci.org/juju4/ansible-macos-apps-install)
# MacOS Applications installation ansible role

Ansible role to setup a list of mac applications be it dmg or pkg format.
It is idempotent and will only update application if version is higher than current one (if relevant fields are present)

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 1.9
 * 2.0
 * 2.2

### Operating systems

Target MacOS 10.11, 10.12

## Example Playbook

Just include this role in your list.
For example

```
- host: all
  roles:
    - juju4.macos-apps-install
```

## Variables

See test/integration/default/default.yml for examples of applications installation.

```
verbose: false
install_archives: /private/var/_install

macos_apps_install_list:
    - { u: 'https://download.mozilla.org/?product=firefox-50.1.0-SSL&os=osx&lang=fr', f: 'firefox-50.1.0-SSL-fr.dmg', v: true, s: '63c561051b1846f110ea60446a6578991a84d169e07dcfe1f4ea35aa4ca20e2a', n: 'Firefox', t: 'app', version: '50.1.0' }
    - { u: "https://download.documentfoundation.org/libreoffice/stable/5.2.3/mac/x86_64/LibreOffice_5.2.3_MacOS_x86-64.dmg", v: true, s: '7f842a38e00312b6283341af3323433558bc155495cf54206dc88a5dd3abe277', n: 'LibreOffice', t: 'app', version: '5.2.3' }
    - { u: 'http://download.virtualbox.org/virtualbox/5.1.10/VirtualBox-5.1.10-112026-OSX.dmg', v: false, s: '3500f07be9f5e27a08c00cc626981230bdaf639f6fad0b82ecf731a0580cccef', n: 'VirtualBox', t: 'pkg', version: '5.1.10' }


```


## Continuous integration

This role has a travis basic test (for github) only.


## Troubleshooting & Known issues


## License

BSD 2-clause

