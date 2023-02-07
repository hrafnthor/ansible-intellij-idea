Role Name
=========

This role will download, extract and configure any number of Jetbrains Intellij IDEA.

Requirements / Dependencies
------------

There are no requirements nor dependencies.

Setup
-----

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a requirements.yml file in the root directory of the playbook being worked on.

2. Add the following definition inside the requirements.yml file:

```yaml
- name: hth-intellij-idea
  src: https://github.com/hrafnthor/ansible-android-studio.git
  scm: git
````

3. Install the requirements by executing

```yaml
ansible-galaxy install -r .requirements.yml
```

This will allow any playbook run from this machine to use the role hth-android-studio

Role Variables
--------------

All variables are optional unless otherwise stated.

##### Default role variables

`intellij_path`:    [string]: (required) The path at which intellij clients will be extracted to. Uses the client `edition` and `version` in determining the directory to extract to. Defaults to `/opt/jetbrains/intellij`

###### Example

When extracting a community edition of Intellij with version 2020.2.2 the path will be:

`<intellij_path>/community/2020.2.2/`


##### User defined variables

`intellij_clients` [array]: Contains a list of client definitions that will be downloaded and extracted onto the host machine. While not required by the role, it will not do much unless given a list of clients to process.

Information on each available client can be found at Jetbrains archive [here](https://www.jetbrains.com/idea/download/other.html).

There are a few fields that each object can have, and they are:

- `version`:    [string] (required) the version number of the IDE
- `checksum`:   [string] (required) the checksum of the archive being downloaded

    - the checksum can be gotten by navigating to the download url of the archive and appending `.sha256` to the url. For example `https://download.jetbrains.com/idea/ideaIC-2022.3.2.tar.gz.sha256`.

- edition:      [enum] (required) the edition of the IDE. Can either be `community` or `ultimate`
- desktop:      [boolean] Indicates if a desktop entry should be created for the IDE.


Example Playbook
----------------

Here is an example for downloading two different versions of the IDE, where one will have a desktop entry created while the other one will not.

```yaml
 - hosts: all
      vars:
        - intellij_clients:
            - version: "2022.3.2"
              checksum: "02bc35281eb4e1285eeb9d797ec2b31ec7370e320ad0e89f6f1fa704d78ec4bf"
              edition: community
              desktop: false
            - version: "2022.3.2"
              checksum: "6fa3aff1c730bb79bf3e2e29edcce6d4cdbccfa631524c6253de518be6b6f3d2"
              edition: ultimate
              desktop: true
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
