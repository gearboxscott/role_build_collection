role_build_collection
=========

This role will update and maintain Ansible Collections using ansible. It will do the following:

* Initialize the collection if not present.
* Initialize the git repository.
* Pull in roles using `git submodule add`.
* Synchronize roles via the submodule.
* Update the collection repository
* Build the collection so it can be published to Ansible Automation Private Automation Hub (PAH).
* Publish the collection to PAH.

Requirements
------------

* Share your ssh public key `~/.ssh/id_rsa.pub` with the version control service that is storing your roles as single repositories at the global level for your account. This will allow clone of role repositories during building our collections on your workstation.

Role Variables
--------------

Create a inventory like the example below and name it by the namespace and collection_name. Here is example names:

  * `spacely-sprockets-general-inv.yml` for collection `spacelysprockets.general`
  * `spacely-sprockets-network-inv.yml` for collection `spacelysprockets.network`
  * `spacely-sprockets-cloud-inv.yml` for collection `spacelysprockets.cloud`

> NOTE: The only option to pull roles available in this role is by `ssh` with git.

| Variable | Type | Required | Table Variable | Defaults | Comments |
|-|:-:|:-:|:-:|-|-|
| collection_version | String | Yes | No | `1.0.0` | Semantic versioning format<br><br>Example: 1.0.0
| organization | String | Yes | No | None | The organization on the version control server.
| project | String | Yes | No | None | The project area on the version control server.
| git_method | String | Yes | No | `ssh` | Connection type for repositories.
| git_ssh_base | String | Yes | No | None | Base connection path for repositories.
| **Flag Files** ||||||
| collection_check_flag_file | String | Yes | No | None | Check for this file to show that collection initialization has been performed if not this role will initialize the collection.
| git_check_flag_file | String | Yes | No | None | Check for this file to show that `git init` has been performed if not this role will initialize git.
| submodule_check_flag_file | String | Yes | No | None | Check for this file to show that `.gitmodules` has been create if not this role will create it.
| **galaxy.yml parameters** ||||||
| author | String | Yes | No | None | Name of the author of the collection.<br><br>Format: `First Last <email address>`
| description | String | Yes | No | None | Description of the collection.
| license | String | Yes | No | `private` | Name of the license file for the collection's usage.
| dependencies | List / element=string  | Yes | No | None | List of dependencies for the collection.
| tags | List / element=string  | Yes | No | None | List of search tags for the collection.
| build_ignore | List / element=string  | Yes | No | `ansible.cfg`<br>`release.yml`<br>`.github`<br>`.ansiblelint.yml`<br>`.yamllint.yml` | List of files to ignore for the collection.
| **Collection Roles parameters** ||||||
| collection_path | String  | Yes | No | None | Path to your collections build directory.<br><br>Example: `'{{ ansible_env.HOME }}/ansible/CODE/collections'`
| collection_namespace | String  | Yes | No | None | Namespace of your collection, which is usually the name of your corporation. Example: `spacely_sprockets`
| collection_name | String  | Yes | No | None | Name of your collection, which is usually the name that best describes the purpose of the roles in the collection.<br><br>Example: `general`
| repo_list | List / elements=string | Yes | Yes | None | Start of table.
| name | String | Yes | No | None | Name of the repository to add.<br><br>Examp.e:<br><br> `- name: role_lnx_baseos`
| version | String | Yes | No | None | Branch or version tag for the role respositories being added to the collection.<br><br>Paired with the `name` variable on the previous line.<br><br>Example:<br><br>`   version: main`

Example of variables:

```yaml
  collection_version: 1.0.0
    # Version Control System
    organization: your_organization_in_version_control
    project: your_project_in_version_control
    git_method: ssh
    git_ssh_base: 'ssh.dev.azure.com:v3'
    git_http_base: 'dev.azure.com'
    # Flag Files
    git_check_flag_file: '.git/config'
    submodule_check_flag_file: '.gitmodules'
    collection_check_flag_file: galaxy.yml
    # Galaxy Files
    author: 'John Smith <jsmith@some_domain.com>'
    description: 'roles for building, configuring, and maintaining configurations for servers'
    readme: README.md
    license: 'private'
    dependencies:
      - community.general
    tags: []
    build_ignore:
      - ansible.cfg
      - release.yml
      - .github
      - .ansiblelint.yml
      - .yamllint.yml
    # Roles
    collection_path: ''{{ ansible_env.HOME }}/ansible/CODE/collections'
    collection_namespace: your_organization
    collection_name: general
    repo_list:
      - name: role_lnx_accounts
        version: main
      - name: role_lnx_base
        version: main
      - name: role_lnx_downloads
        version: main
```

Dependencies
------------

None.

Example Playbook
----------------

None.

Collection Versions
-------------------

  Once you upload a version of a collection, you cannot delete or modify that version. Ensure that everything looks okay before uploading. The only way to change a collection is to release a new version. The latest version of a collection (by highest version number) will be the version displayed everywhere in Galaxy; however, users will still be able to download older versions.

  Collection versions use [Semantic Versioning](https://semver.org/) for version numbers. Please read the official documentation for details and examples. In summary:

  * Increment major (for example: x in x.y.z) version number for an incompatible API change.
  * Increment minor (for example: y in x.y.z) version number for new functionality in a backwards compatible manner.
  * Increment patch (for example: z in x.y.z) version number for backwards compatible bug fixes.

References
----------

* [Semantic Versioning 2.0.0](https://semver.org/)
* [On the Practice of Semantic Versioning for Ansible Galaxy Roles: An Empirical Study and a Change Classification Model](https://www.sciencedirect.com/science/article/abs/pii/S0164121221001564)

License
-------

GPL

Author Information
------------------

Scott Parker <sparker@redhat.com>
