Role Name
=========

This role will download, extract and configure any number of Jetbrains Intellij IDEA.

Requirements / Dependencies
------------

This role is dependent on `ansible.utils.jsonschema`.

Setup
-----

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a requirements.yml file in the root directory of the playbook being worked on.

2. Add the following definition inside the requirements.yml file:

```yaml
- name: hth-intellij-idea
  src: https://github.com/hrafnthor/ansible-intellij-idea.git
  scm: git
````

3. Install the requirements by executing

```yaml
ansible-galaxy install -r .requirements.yml
```

This will allow any playbook run from this machine to use the role hth-intellij-idea


##### Scheme

```yaml
intellij:
  location:                           Requirements in object only apply if object is defined
    path: [string]                    The path to where client installation should take place. Defaults to using the value in 'hth_intellij_default_path'.
    owner: [string]                   The owner to set for the installation directory. Defaults to 'root'.
    group: [string]                   The owner group to set for the installation directory. Defaults to 'root'.
    mode: [string]                    The directory permissions for the installation path. Defaults to '0755'.
  clients:                            Array of clients
    - version: [string]               [required]  the version code of the client that should be installed
      checksum: [string]              [depends]   checksum of the client to install. Only required when installing
      remove: [boolean]               Indicates if the client should be removed. Also removes the desktop entry.
      desktop:                        Optional configuration for desktop file creation
        remove: [boolean]             Indicates if the desktop file should be removed.
        name: [string]                Optional custom name for the client desktop file. Defaults to 'Intellij-<version>'.
        keywords: [array]             Optional list of keywords that will be added to the desktop file for serviceability.
```

Information on each available client can be found at Jetbrains archive [here](https://www.jetbrains.com/idea/download/other.html).

The checksum can be gotten by navigating to the download url of the archive and appending `.sha256` to the url. For example `https://download.jetbrains.com/idea/idea-2025.3.tar.gz.sha256`.

##### Defaults

The following default variables are also available for modification:

`hth_intellij_default_path`:

The default location to where client installations should be placed. Defaults to `'/opt/jetbrains/intellij'`.

`hth_intellij_default_desktop_entry_path`:

The default location to where desktop file entries are created. Defaults to `'/usr/share/applications'`.

`hth_intellij_default_host`:

The default host to download archive files from. Defaults to `https://download.jetbrains.com`.

`hth_intellij_default_url_path`:

The default url path that is appended to the host when downloading archives. Defaults to `/idea/`.

`hth_intellij_default_cleanup_on_install`:

Indicates if the archive file should be deleted after installation has finished. Defaults to `true`.


##### Example

```yaml
 - hosts: all
      vars:
        intellij:
          location:
            group: "developers"
            mode: "0775"
          clients:
            - version: "2025.3"
              checksum: "...."
              desktop:
                name: "intellij-latest"
                keywords:
                  - java
                  - development
                  - ide
                  - jetbrains
      roles:
         - hth-intellij-idea
```

License
-------

Apache License 2.0. See attached license file.

Author Information
------------------

Hrafn Thorvaldsson.
http://www.hth.is
