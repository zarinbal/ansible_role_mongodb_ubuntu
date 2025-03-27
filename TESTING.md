# Testing MongoDB Ansible Role

This document provides comprehensive information about testing the MongoDB Ansible role.

## Prerequisites

1. Install Python 3.8 or higher
2. Install Docker for running test containers
3. Set up a virtual environment:
   ```bash
   # Create a virtual environment
   python3 -m venv .venv

   # Activate the virtual environment
   source .venv/bin/activate

   # Install required packages
   pip install -r requirements-dev.txt
   ```

## Running Tests

### Using Molecule (Recommended)

```bash
# Make sure your virtual environment is activated
source .venv/bin/activate

# Run all tests
molecule test

# Run specific test scenarios
molecule test --scenario-name default

# Run tests with specific playbook
MOLECULE_PLAYBOOK=tests/test.yml molecule test
```

### Manual Testing with Vagrant

```bash
# Create test VMs
vagrant up

# Run the role
ansible-playbook -i vagrant_inventory playbooks/test.yml

# Clean up
vagrant destroy -f
```

### Testing with Docker

```bash
# Build test container
docker build -t mongodb-test -f tests/Dockerfile .

# Run tests
docker run -it mongodb-test ansible-playbook /tests/test.yml
```

### Testing Different MongoDB Versions

```yaml
# In your test playbook
- hosts: all
  vars:
    mongo_version: "8.0"  # Test MongoDB 8.0
    mongo_edition: org
  roles:
    - ansible_role_mongodb_ubuntu
```

## Test Scenarios

The test suite includes the following scenarios:

1. **Default Scenario**
   - Tests MongoDB installation on Ubuntu 22.04 and 24.04
   - Verifies service status and configuration
   - Checks MongoDB version and connectivity
   - Validates file permissions and ownership

2. **Version Compatibility**
   - Tests different MongoDB versions (6.0, 7.0, 8.0)
   - Verifies version-specific configurations
   - Checks backward compatibility

3. **Security Testing**
   - Validates authentication setup
   - Checks file permissions
   - Verifies network security settings

## Test Files Structure

```
.
├── tests/
│   ├── test.yml              # Main test playbook
│   └── Dockerfile            # Docker test container
├── molecule/
│   └── default/
│       ├── molecule.yml      # Molecule configuration
│       ├── prepare.yml       # Test environment setup
│       └── verify.yml        # Verification tasks
└── requirements-dev.txt      # Development dependencies
```

## Troubleshooting

### Docker Issues

```bash
# Clean up Docker resources
molecule destroy
docker system prune -f
```

### Virtual Environment Issues

```bash
# Recreate virtual environment
rm -rf .venv
python -m venv .venv
source .venv/bin/activate
pip install -r requirements-dev.txt
```

### Molecule Issues

```bash
# Reset Molecule state
molecule destroy
molecule prepare
molecule converge
``` 