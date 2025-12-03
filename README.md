# network-cisco-ios

Purpose
- Cisco IOS automation: vendor-specific roles (config, backup, compliance), tests, and CI.
- Integrates with platform/vendor_abstraction and NetBox dynamic inventory; executed via AWX.

Quickstart
1. Install collections: `ansible-galaxy collection install -r collections/requirements.yml`
2. Install platform roles: `ansible-galaxy role install -r requirements.yml -p ./roles`
3. Run validation locally: `ansible-playbook playbooks/validate.yml -i inventories/lab_static.yml`
4. AWX: create Project pointing to this repo and Inventory Source using NetBox.

Branching and Releases
- Branches: `main`, `develop`, `feature/<ticket>`
- PRs require CI green and at least one reviewer.
- Tag releases using semantic versioning (vMAJOR.MINOR.PATCH).

Contacts
- Owners: B S Dada Khalandar
