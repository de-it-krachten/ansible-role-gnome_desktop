[![CI](https://github.com/de-it-krachten/ansible-role-gnome_desktop/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-gnome_desktop/actions?query=workflow%3ACI)


# ansible-role-gnome_desktop

Install the Gnome 3.x desktop
Primary goal of this role, is to mke it easy to install the Gnome desktop onto headless Vagrant boxes,
when testing code onto vm's that require a desktop environment. 



## Dependencies

#### Roles
None

#### Collections
- community.general

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS<sup>1</sup>
- Fedora 36
- Fedora 37

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# OS packages that need to be absent, as they block gnome installation
gnome_desktop_blocking_packages: []

# Only install minimal set of packages
gnome_desktop_minimal: false

# List of packages for minimal desktop
gnome_desktop_packages_minimal: []

# Package mode (switch to 'install' for less output)
gnome_desktop_package_mode: 'install-verbose'

# Should wayland be activated
gnome_desktop_wayland: true

# Configure auto login
gnome_desktop_autologin_enable: false

# User that should be used for autologin
gnome_desktop_autologin: ''

# Disable screen lock
gnome_desktop_lock_disable: false

# Timeout (in seconds) before locking
gnome_desktop_lock_timeout: '900'

# GDM configuration file to use
gnome_desktop_gdm_conf: "{{ ansible_facts.ansible_local.dm.conf }}"

# GNOME settings
gnome_desktop_settings:
  - key: /org/gnome/desktop/session/idle-delay
    value: '{{ gnome_desktop_lock_timeout }}'
  - key: /org/gnome/desktop/screensaver/lock-delay
    value: '{{ gnome_desktop_lock_timeout }}'
  - key: /org/gnome/desktop/screensaver/lock-enabled
    value: "{{ 'false' if gnome_desktop_lock_disable | bool else 'true' }}"
</pre></code>


### vars/family-RedHat-9.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  - "@graphical-server-environment"
  - python3-psutil
</pre></code>

### vars/Ubuntu-1804.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  - ubuntu-desktop
  - python3-psutil

# List of package / package groups to install - minimal
gnome_desktop_packages_minimal: []
</pre></code>

### vars/Ubuntu.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  - ubuntu-desktop
  - python3-psutil

# List of package / package groups to install - minimal
gnome_desktop_packages_minimal:
  - ubuntu-desktop-minimal
  - python3-psutil
</pre></code>

### vars/family-RedHat.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  - "@gnome-desktop"
  - python3-psutil
</pre></code>

### vars/Debian.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  - task-gnome-desktop
  - python3-psutil
</pre></code>

### vars/Fedora.yml
<pre><code>
# List of package known to block gnome installation
gnome_desktop_blocking_packages: []

# List of package / package groups to install
gnome_desktop_packages:
  # - "@workstation-product-environment"
  - "@gnome-desktop"
  - python3-psutil
</pre></code>



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'gnome_desktop'
  hosts: all
  become: "yes"
  vars:
    hashicorp_product: vagrant
  roles:
    - deitkrachten.showinfo
  tasks:
    - name: Include role 'gnome_desktop'
      ansible.builtin.include_role:
        name: gnome_desktop
</pre></code>
