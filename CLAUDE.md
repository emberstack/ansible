# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains **EmberStack Ansible Collections**, providing enterprise-grade automation for network infrastructure. The repository is organized to support multiple collections under the EmberStack namespace.

## Key Commands

```bash
# Install specific collection in development mode
ansible-galaxy collection install ./src/fortios --force

# Run sanity tests
ansible-test sanity

# Run integration tests  
ansible-test integration

# Install from source
ansible-galaxy collection install git+https://github.com/emberstack/ansible.git#/src/fortios
```

## Architecture and Structure

### Repository Structure
- **Collections Path**: `src/`
- **Available Collections**:
  - `fortios` - Fortinet FortiOS devices (FortiGate, FortiAP, etc.)
- **Namespace**: `emberstack`

### FortiGate Role Architecture

The FortiGate role (`src/fortios/roles/fortigate/`) uses a highly modular architecture:

1. **Task Organization**: Each FortiGate feature has its own task file in `tasks/`:
   - `addresses.yaml` - Firewall address objects
   - `interfaces.yaml` - Network interfaces
   - `policies.yaml` - Security policies
   - `sdwan.yaml` - SD-WAN configuration
   - `ipsec.yaml` - IPSec VPN tunnels
   - `ssl_vpn.yaml` - SSL VPN configuration
   - 30+ more task files for comprehensive coverage

2. **Version Support**: Version-specific configurations in `vars/versions/`:
   - `7.4.yaml` - FortiOS 7.4 compatibility

3. **Variable Structure**: All variables follow pattern `fortigate_[category]_[resources]`:
   - `fortigate_firewall_addresses`
   - `fortigate_vpn_tunnels`
   - `fortigate_system_settings`

### Key Design Patterns (from docs/PATTERNS.md)

1. **Basic Resource Configuration**:
   ```yaml
   - name: Configure [resource] - {{ item.[key].name }}
     fortinet.fortios.fortios_[module]:
       state: "{{ item.state | default(fortigate_default_state) }}"
       access_token: "{{ access_token }}"
       vdom: "{{ item.vdom | default(vdom | default(fortigate_vdom)) }}"
   ```

2. **Loop Control**: Always use descriptive labels and custom loop variables when needed
3. **Error Handling**: Standard retry pattern with `fortigate_connection_retries` and `fortigate_connection_delay`
4. **VDOM Support**: All resources support VDOM with fallback to default

## Dependencies

- Ansible 2.15+
- Python 3.9+
- Required collections:
  - `fortinet.fortios` >= 2.4.2
  - `ansible.netcommon` >= 8.2.0

## Development Guidelines

1. **Task Files**: One file per logical feature group
2. **Variable Naming**: Follow `fortigate_[category]_[resources]` pattern
3. **Task Names**: Must be descriptive with resource identification
4. **Module Names**: Always use FQCN (`fortinet.fortios.fortios_*`)
5. **Default Values**: Use cascade pattern for defaults
6. **When Conditions**: Always check both defined and length

## Testing Approach

- Unit tests: Not yet implemented
- Integration tests: Use `ansible-test integration`
- Sanity tests: Use `ansible-test sanity`
- Manual testing: Required for FortiGate device interactions

## Common Tasks

### Adding New FortiGate Feature
1. Create new task file in `src/fortios/roles/fortigate/tasks/`
2. Follow patterns from `docs/PATTERNS.md`
3. Add include in `main.yaml` with appropriate tags
4. Define variables in `defaults/main.yaml`
5. Test with example playbooks

### Modifying Existing Features
1. Locate task file in `src/fortios/roles/fortigate/tasks/`
2. Follow existing patterns in the file
3. Maintain backward compatibility
4. Update defaults if adding new parameters

### Version-Specific Changes
1. Check `vars/versions/` for version files
2. Add version-specific overrides as needed
3. Test with FortiOS 7.4

### Adding New Collections
1. Create new directory under `src/[collection_name]`
2. Add `galaxy.yml` with proper namespace and collection name
3. Create standard structure: `roles/`, `plugins/`, `playbooks/`, `docs/`, `tests/`
4. Update repository README with new collection information

## Important Notes

- This is infrastructure automation code for defensive security purposes
- All tasks support both individual resource and list configurations
- VDOM support is built into all applicable resources
- Connection retry logic is standard across all tasks
- Commercial support available at support@emberstack.com