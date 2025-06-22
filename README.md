# EmberStack Ansible Collections

Enterprise-grade Ansible collections for network infrastructure automation.

## Overview

This repository contains multiple Ansible collections under the EmberStack namespace, providing comprehensive automation solutions for various network platforms and infrastructure components.

## Available Collections

### emberstack.fortios
Comprehensive automation for Fortinet FortiOS devices including FortiGate firewalls.

**Features:**
- Complete FortiGate configuration management
- Support for FortiOS 7.4
- VDOM support
- SD-WAN configuration
- VPN management (IPSec, SSL)
- And much more...

[View FortiOS Collection Documentation](src/fortios/)

## Installation

### Install All Collections
```bash
# Clone the repository
git clone https://github.com/emberstack/es.fx.ansible.git
cd es.fx.ansible

# Install all collections
for collection in src/*/; do
  ansible-galaxy collection install "$collection" --force
done
```

### Install Specific Collection
```bash
# Install FortiOS collection
ansible-galaxy collection install ./src/fortios

# Or directly from GitHub
ansible-galaxy collection install git+https://github.com/emberstack/es.fx.ansible.git#/src/fortios
```

### Using as Git Submodule
```bash
# Add repository as submodule
git submodule add https://github.com/emberstack/es.fx.ansible.git ansible_collections_repo

# Use collections from the submodule
export ANSIBLE_COLLECTIONS_PATH="${PWD}/ansible_collections_repo/src:~/.ansible/collections"
```

## Repository Structure

```
es.fx.ansible/
├── src/
│   └── fortios/             # FortiOS collection
│       ├── galaxy.yml
│       ├── README.md
│       ├── roles/
│       ├── plugins/
│       ├── playbooks/
│       └── docs/
├── README.md                    # This file
├── LICENSE                      # Repository license
└── CLAUDE.md                    # Development documentation
```

## Requirements

- Ansible 2.9 or higher
- Python 3.6 or higher
- Collection-specific requirements (see individual collection documentation)

## Usage Example

```yaml
---
- name: Configure network infrastructure
  hosts: network_devices
  collections:
    - emberstack.fortios
    
  tasks:
    - name: Configure FortiGate firewall
      include_role:
        name: fortigate
      when: device_type == "fortigate"
```

## Development

### Setting Up Development Environment
```bash
# Clone the repository
git clone https://github.com/emberstack/es.fx.ansible.git
cd es.fx.ansible

# Install collections for development
for collection in src/*/; do
  ansible-galaxy collection install "$collection" --force
done
```

### Running Tests
```bash
# Test specific collection
cd src/fortios
ansible-test sanity
ansible-test integration
```

## Contributing

We welcome contributions! Please see our contributing guidelines for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/emberstack/es.fx.ansible/issues)
- **Discussions**: [GitHub Discussions](https://github.com/emberstack/es.fx.ansible/discussions)

## License

MIT License - see [LICENSE](LICENSE) file for details.
