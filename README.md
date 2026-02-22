[![CircleCI](https://circleci.com/gh/ansible-roles-mamono210/vnc_server_install/tree/main.svg?style=svg)](https://circleci.com/gh/ansible-roles-mamono210/vnc_server_install/tree/main)

Role Description
================

Installs TigerVNC server packages and (optionally) GUI components for Enterprise Linux 8/9 (e.g., CentOS Stream 9).

This role is **Golden Image safe**:
- It installs system-level packages only.
- It does NOT create users.
- It does NOT configure `~/.vnc`.
- It does NOT set VNC passwords.
- It does NOT start or enable VNC services.

Per-user configuration must be handled by a separate role
(e.g. `mamono210.vnc_user_config`) and executed **after VM boot**.

---

Requirements
------------

- Enterprise Linux 9 compatible system
- `dnf` available
- EPEL may be required depending on environment

Example:
```yaml
- role: robertdebock.epel
````

---

## Role Variables

### vnc_install_gui

Whether to install GUI group (`@Server with GUI`).

Default:

```yaml
vnc_install_gui: true
```

If you want a minimal image without full desktop environment:

```yaml
vnc_install_gui: false
```

---

### vnc_gui_group

DNF group to install when `vnc_install_gui` is enabled.

Default:

```yaml
vnc_gui_group: "@Server with GUI"
```

---

### vnc_install_packages

List of TigerVNC-related packages to install.

Default:

```yaml
vnc_install_packages:
  - tigervnc-server
```

---

## Dependencies

None.

This role intentionally does not depend on any user management role.

---

## Example Playbook (Golden Image / Packer)

```yaml
---
- hosts: all
  become: true

  roles:
    - role: robertdebock.epel
    - role: mamono210.vnc_install
      vars:
        vnc_install_gui: true
```

This playbook installs required system packages only.
It does not configure any user-specific VNC settings.

---

## Operational Model

This role is designed for a two-phase deployment model:

### Phase 1: Image Build (Packer)

* Install OS packages
* Install TigerVNC
* Do NOT configure users
* Do NOT configure passwords
* Do NOT start services

### Phase 2: VM Runtime Configuration

Run a separate role (e.g. `mamono210.vnc_user_config`) to:

* Configure `~/.vnc`
* Set VNC password
* Map display to user
* Enable and start `vncserver@.service`

This separation prevents:

* Conflicts with cloud metadata users (e.g. GCE)
* Password leakage into Golden Images
* SSH login failures caused by user collisions

---

## License

BSD
