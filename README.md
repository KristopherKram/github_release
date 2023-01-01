kristopherkram.github_release
=========

Installs and updates assets from GitHub along with their symbolic links 

Requirements
------------

Passing the archive role variable requires that the valid unarchiving software for ansible.builtin.unarchive is present. See the role variables section for more information.

Warning
-------

This playbook will delete folders in the `/opt/` path by default. Please make sure that any software you install does not match an existing directory name. For example if you were to install krew via this role it would delete everything in `/opt/krew/`

Role Variables
--------------

* temp_download_path: 
  * Defines where the asset should be downloaded first before it is isntalled or extracted
  * Default: `/tmp/`
* install_path:
  * Defines where the asset should be installed to. The name of the GitHub project is appended to create a unique directory.
  * Default: `/opt/`
* install_path_owner:
  * Updates the file permissions of the installed software to be owned by this user.
  * Default: `root`
* install_path_group:
  * Updates the file permissions of the installed software to be owned by this group.
  * Default: `root`
* install_path_mode: 
  * Updates the file permissions of the installed software to have this file access.
  * Default: `0755`
* install_info_owner:
  * Updates the file permissions of .install-info.json to be owned by this user.
  * Default: `root`
* install_info_group:
  * Updates the file permissions of .install-info.json to be owned by this group.
  * Default: `root`
* install_info_mode: 
  * Updates the file permissions of .install-info.json to have this file access.
  * Default: `0755`
* uninstall:
  * Uninstalls the software by deleting the directory. This will not remove any links that were created via the asset_links arguments.
  * Default: `false`
* github_user:
  * The GitHub user we wish to download the software from. In the case of https://github.com/kubernetes-sigs/kind/releases/download/v0.17.0/kind-linux-amd64 the user is `kubernetes-sigs`.
* github_repo:
  * The GitHub repo we wish to download the software from. In the case of https://github.com/kubernetes-sigs/kind/releases/download/v0.17.0/kind-linux-amd64 the repo is `kind`.
* version:
  * The version of the software to install. In the case of https://github.com/kubernetes-sigs/kind/releases/download/v0.17.0/kind-linux-amd64 the version is `v0.17.0`.
* asset:
  * The asset of the software to install. In the case of https://github.com/kubernetes-sigs/kind/releases/download/v0.17.0/kind-linux-amd64 the asset is `kind-linux-amd64`.
* archive:
  * This argument will extract the download asset after it is downloaded. Note that this requires that the valid unarchiving software for ansible.builtin.unarchive is present. See the notes for unarchive for more information [https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html).
* asset_links:
  * Creates a symbolic link after the software is installed. Multiple links can be defined. Note that the links are not cleaned up after uninstall. The configuration below creates a symbolic link in /usr/local/bin/krew pointing to /opt/krew/krew-linux_amd64.
  ```yaml
  - src: krew-linux_amd64
        dest: /usr/local/bin/krew
        owner: root
        group: root
        mode: "0755"
  ```


Example Playbook
----------------
```yaml
- name: "Install krew"
  ansible.builtin.include_role:
    name: "kristopherkram.github_release"
  vars:
    github_user: kubernetes-sigs
    github_repo: krew
    version: v0.4.3
    asset: krew-linux_amd64.tar.gz
    archive: true
    asset_links:
      - src: krew-linux_amd64
        dest: /usr/local/bin/krew
        owner: root
        group: root
        mode: "0755"

- name: "Install kind"
  ansible.builtin.include_role:
    name: "kristopherkram.github_release"
  vars:
    github_user: kubernetes-sigs
    github_repo: kind
    version: v0.17.0
    asset: kind-linux-amd64
    asset_links:
      - src: kind-linux-amd64
        dest: /usr/local/bin/kind-linux-amd64
        owner: root
        group: root
        mode: "0755"
```
Issues
------
* Links are not cleaned up on uninstall or upgrade
* This playbook will delete folders in the `/opt/` path by default. Please make sure that any software you install does not match an existing directory name. For example if you were to install krew via this role it would delete everything in `/opt/krew/`

License
-------

GPL-3.0-or-later

