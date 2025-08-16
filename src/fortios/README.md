# EmberStack FortiOS Collection

A comprehensive Ansible collection for automating Fortinet FortiOS devices, starting with enterprise-grade FortiGate firewall management.

## Description

The EmberStack FortiOS collection (`emberstack.fortios`) provides production-ready Ansible roles for automating FortiOS-based network infrastructure. The collection currently focuses on comprehensive FortiGate firewall management with plans to expand to other FortiOS devices.

## Requirements

- Ansible 2.9 or higher
- Python 3.6 or higher
- FortiOS 7.4
- Required Ansible collections:
  - `fortinet.fortios` >= 2.4.0
  - `ansible.netcommon` >= 2.0.0

## Installation

### Install from Source
```bash
ansible-galaxy collection install git+https://github.com/emberstack/ansible.git#/src/fortios
```

### Install from Local Directory
```bash
# From the collection directory
cd src/fortios
ansible-galaxy collection install . --force
```

## Included Content

### Roles

#### fortigate
Comprehensive FortiGate firewall configuration management.

**Features:**
- System configuration (hostname, DNS, NTP, certificates)
- Network configuration (interfaces, zones, VLANs, routing)
- Security policies and objects (addresses, services, policies)
- VPN configuration (IPSec, SSL VPN)
- SD-WAN configuration
- Wireless controller (FortiAP management)
- High availability and clustering
- Complete VDOM support

**Supported FortiOS Version:**
- 7.4.x

## Usage

### Basic Playbook
```yaml
---
- name: Configure FortiGate Firewall
  hosts: fortigates
  collections:
    - emberstack.fortios
  
  roles:
    - fortigate
```

### Connection Configuration
```yaml
---
- name: Configure FortiGate
  hosts: fortigates
  gather_facts: no
  connection: httpapi
  collections:
    - emberstack.fortios
    - fortinet.fortios
  
  vars:
    ansible_network_os: fortinet.fortios.fortios
    ansible_httpapi_use_ssl: yes
    ansible_httpapi_validate_certs: no
    ansible_httpapi_port: 443
    
  roles:
    - fortigate
```

### Using with Variables
```yaml
---
- name: Configure FortiGate with Custom Settings
  hosts: fortigates
  collections:
    - emberstack.fortios
    
  vars:
    fortigate_system_settings:
      hostname: "firewall-01"
      timezone: "America/New_York"
    
    fortigate_firewall_addresses:
      - firewall_address:
          name: "web_server"
          subnet: "192.168.100.10/32"
          type: "ipmask"
    
  roles:
    - fortigate
```

## Role Variables

All FortiGate role variables follow the pattern `fortigate_[category]_[resources]`:

- `fortigate_system_settings` - System configuration
- `fortigate_interfaces` - Network interfaces
- `fortigate_firewall_addresses` - Address objects
- `fortigate_firewall_policies` - Security policies
- `fortigate_vpn_tunnels` - VPN configuration
- `fortigate_sdwan_zones` - SD-WAN zones
- And many more...

See the [fortigate role documentation](roles/fortigate/README.md) for complete variable reference.

## Examples

Example playbooks are provided in the `playbooks/examples/` directory:

- `fortigate_connect_only.yml` - Connection test without changes

## Testing

### Running Tests
```bash
# Run sanity tests
ansible-test sanity

# Run integration tests
ansible-test integration
```

## Support

- **Repository**: [GitHub](https://github.com/emberstack/ansible)
- **Issues**: [GitHub Issues](https://github.com/emberstack/ansible/issues)

## License

MIT License - see LICENSE file for details.

## Authors

- EmberStack Team