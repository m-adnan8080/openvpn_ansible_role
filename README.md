
# OpenVPN Ansible Role

This Ansible role helps to automate the setup and management of an OpenVPN server. It includes tasks to configure the OpenVPN server, manage client accounts, and generate client configuration files (`.ovpn`).

## Features

- Set up an OpenVPN server on a Linux host.
- Manage OpenVPN clients and automatically generate `.ovpn` files for each user.
- Tested on Debian-based systems and RedHat based Linux Distributions.
- Easily customizable using Ansible variables.

## Requirements

- Ansible 2.9 or later.
- Supported Linux distributions:
  - Debian 12
  - Ubuntu 22.04
  - CentOS 8
  - Rocky 9
  - Fedora
  - RedHat

## Role Variables

These variables can be customized in your playbook or in the `group_vars/host_vars` file.

| Variable                    | Description                                                           | Default Value |
| ---------------------------- | --------------------------------------------------------------------- | ------------- |
| `openvpn_port`               | The port on which OpenVPN will run.                                   | `1194`        |
| `openvpn_protocol`           | The protocol for OpenVPN (e.g., `udp` or `tcp`).                      | `udp`         |
| `openvpn_network`            | The network subnet for VPN clients.                                   | `10.8.0.0`    |
| `openvpn_netmask`            | The netmask for the VPN network.                                      | `255.255.255.0` |
| `openvpn_client_config_path` | Path where the `.ovpn` client files will be stored.                   | `/etc/openvpn/client-configs` |
| `openvpn_server_cn`          | Common Name (CN) for the OpenVPN server certificate.                  | `vpn-server`  |
| `openvpn_user_list`          | List of clients to be added to the VPN, defined in a variable file.   | []            |

## Usage

### 1. OpenVPN Server Setup

To set up the OpenVPN server, you can use the provided playbook `openvpn-server-setup.yaml`:

```yaml
---
- hosts: vpn-server
  become: true
  roles:
    - openvpn
```

This playbook will install and configure the OpenVPN server on the target host.

### 2. Add VPN Clients

To add new VPN clients, use the `add-client-account.yaml` playbook. You need to specify a list of users in a variable file (`users.yml`), like this:

```yaml
openvpn_user_list:
  - username1
  - username2
  - username3
```

Run the playbook to generate `.ovpn` configuration files for each user:

```bash
ansible-playbook add-client-account.yaml -e "@users.yml"
```

The `.ovpn` files will be stored in the directory specified by the `openvpn_client_config_path` variable.

### 3. Download the Client Configuration

Once the clients are created, you can download their `.ovpn` configuration files and distribute them to the users.

## Example Playbooks

### Example for OpenVPN Server Setup

```yaml
---
- name: Install and configure OpenVPN Server
  hosts: openvpn
  become: true
  become_user: root
  become_method: sudo
  vars_files:
    - vars.yaml
  roles:
    - openvpn_server
```

### For Clients addition and removal update the variable in `vars.yaml` file

```yaml
# OpenVPN Client Variables
openvpn_add_users:
  - client1
  - client2

openvpn_remove_users:
  - client3
```

## License

MIT License. See the [LICENSE](LICENSE) file for more details.

## Author

This role is maintained by [m-adnan8080](https://github.com/m-adnan8080).

## Contributions

Contributions are welcome! Please submit a pull request or open an issue if you find a bug or have a feature request.
