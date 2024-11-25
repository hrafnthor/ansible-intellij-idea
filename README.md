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
  location:                           [optional]  Requirements in object only apply if object is defined
    path: [string]                    [required]  path to installation dir (defaults to '/opt/jetbrains/intellij')
    owner: [string]                   [optional]  owner of path dir (defaults to 'root')
    group: [string]                   [optional]  group for path dir (defaults to owner then 'root')
    mode: [string]                    [optional]  chmod for path dir (defaults to 0755)
  clients:                            [optional]  array of clients
    - version: [string]               [required]  the version code of the client that should be installed
      checksum: [string]              [depends]   checksum of the client to install (see below). only when installing
      edition: [community, ultimate]  [required]  depending on the version
      desktop: [boolean]              [optional]  if true will create a desktop entry
      state: [absent, present]        [optional]  if 'absent'; will remove, otherwise install
```

Information on each available client can be found at Jetbrains archive [here](https://www.jetbrains.com/idea/download/other.html).

##### Checksum

The checksum can be gotten by navigating to the download url of the archive and appending `.sha256` to the url. For example `https://download.jetbrains.com/idea/ideaIC-2022.3.2.tar.gz.sha256`.

##### Example

Here is an example for downloading two different versions of the IDE, where one will have a desktop entry created while the other one will not.

```yaml
 - hosts: all
      vars:
        intellij:
          location:
            path: "/opt/jetbrains/intellij"
            owner: "root"
            group: "developers"
            mode: "0755"
          clients:
            - version: "2024.3"
              checksum: "16d4f411b62ddc7747fe11e8fff004bf8d144df4052b8111306fd4cbba8f748c"
              edition: "community"
              desktop: true
            - version: "2022.3.2"
              edition: "community"
              state: absent
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
