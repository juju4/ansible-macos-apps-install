[![Build Status - Master](https://travis-ci.org/juju4/ansible-macos-apps-install.svg?branch=master)](https://travis-ci.org/juju4/ansible-macos-apps-install)
[![Build Status - Devel](https://travis-ci.org/juju4/ansible-macos-apps-install.svg?branch=devel)](https://travis-ci.org/juju4/ansible-macos-apps-install/branches)
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

### soft requirements
GNUtar (gtar) is necessary insead of BSDtar (tar) if you will download non-zip compressed archives

```
brew install gnu-tar
export PTH="/usr/local/opt/gnu-tar/libexec/gnubin:$PATH"
```

## Example Playbook

Just include this role in your list.
For example

```
- host: all
  roles:
    - juju4.macos-apps-install
```

## Variables

See `test/integration/default/default.yml` for examples of applications installation.

```
target_user_id: "username"      # the name of the user we want the apps to belong to
verbose: False
install_archives: ~/Downloads
pip_path: /opt/local/bin/pip    # Homebrew's pip: /opt/local/bin/pip

macos_apps_install_list:
  - { u: 'https://download.mozilla.org/?product=firefox-50.1.0-SSL&os=osx&lang=fr',
      f: 'firefox-50.1.0-SSL-fr.dmg',
      v: True,
      s: '63c561051b1846f110ea60446a6578991a84d169e07dcfe1f4ea35aa4ca20e2a',
      n: 'Firefox',
      t: 'app',
      version: '50.1.0'
    }    
    - { u: 'http://dl.paragon-software.com/demo/ntfsmac15_trial.dmg',
      f: 'ntfsmac15_trial.dmg',
      n: 'NTFS for Mac',
      m: '/Volumes/ParagonFS.ntfs',
      t: 'installer',
      exec_name: 'FSInstaller.app'
    }
  - { u: 'https://some.app.com/download/link', # REQUIRED # Archive Download URL
      f: 'archive_file.dmg',                   # REQUIRED # Archive File Name (how it will be saved with)
      n: 'My Fancy App',                       # REQUIRED # Name of the application also used for the .app file
      exec_name: '',                           # OPTIONAL # Name of the installed file if it is not an .app         
      env: '',                                 # OPTIONAL # Environment variables required by the installer
      version: '1.0.2',                        # OPTIONAL # Required Application Version         
      t: 'app',                                # OPTIONAL # Installer type         
      m: '/Volumes/My Fancy App',              # OPTIONAL # Name of the archive mount point when different than application name or when wanting a custom name
      to: '/Applications',                     # OPTIONAL # Destination name for the application
      versioncmd: '',                          # OPTIONAL # Specific command to retrieve the currently          installed application version
      plist_tag: '',                           # OPTIONAL # Plist tag that identify the application version
      v: True,                                 # OPTIONAL # Toggle file integrity verification after download         
      s: '63c5610...a20e2a',                   # OPTIONAL # Checksum value for file integrity verification
    }

```
## Notes on Veriables
### Installer Types: `t:`
> default: app

* `app`: .app file contained in a DMG archive
* `installer`: .app/exec interactive installer contained in a DMG file
  * requires the use of `exec_name:` to define the name of the installer file  
* `pkg`: .pkg installer contained in a DMG file
* `pkgonly`: .pkg installer NOT contained in a DMG or compressed archive
* `zip`|`compressed_archive`: application contained in any kind of compressed archive

### Exec Name: `exex_name`
This is the name of the script or binary contained in the archive in case it is not a ".app":
* a script (i.e. `script.sh`)
* a no-extension binary (i.e. `my_executable`)
* an installer packages (i.e. `installer.pkg`).

### Version: `version:`
If you do not specify a version number the application will always install and overwrite any existing version.

When a specify a version number is specified:
* the application will be installed if the currently installed version is older
* the application will NOT be  installed if the currently installed version is newer

### Version Command: `versioncmd:`
> default: /usr/libexec/PlistBuddy -c 'print :<plist_tag>'  '/path/to/App/Contents/Info.plist'

This is a command specific to the application to retrieve the currently installed version in case the default command will not be adequate.

### Plist Tag: `plist_tag:`
> default: CFBundleShortVersionString

This is the tag that identify the application version inside the its `.plist` file

### Environments: `env`
Allows to pass environment variables (i.e. API_key ) required for the installer customisations.

```
Example:

env: "env API_KEY={{ prey_apikey | default('') }}"
```

See: http://help.preyproject.com/article/188-prey-unattended-install-for-computers



## Continuous integration

This role has a travis basic test (for github) only.


## Troubleshooting & Known issues


## License

BSD 2-clause
