# 📘 ios_common.restore — Cisco IOS Configuration Restore Role
Author: Bandar Shaik Dada Khalandar — Senior Network Engineer
License: Eclipse Public License - v 2.0

The ios_common.restore role provides a structured, automated workflow for restoring Cisco 
IOS/IOS‑XE device configurations from SFTP‑based backups.

It is designed to support device replacement, configuration recovery, and standardized 
operational workflows using AWX or Ansible Automation Platform.

All sensitive credentials (SFTP, device login, AAA, etc.) are securely managed by AWX/AAP Credential Manager.

No passwords or secrets are stored in this repository.

# 🏗 Role Structure
roles/
  ios_common/
    restore/
      tasks/
        main.yml
        precheck.yml
        download.yml
        apply.yml
        verify.yml
        rollback.yml
        cleanup.yml
      README.md


Each task file represents a distinct phase of the restore lifecycle, enabling clarity, maintainability, and predictable execution.

# 🔄 Workflow Summary
## 1. Precheck
Validates prerequisites before initiating a restore:
- Device reachable via SSH
- SFTP server reachable
- AWX /tmp directory available
## 2. Download
- Lists available backup files on the SFTP server
- Filters only .cfg files matching the device hostname
- Selects the most recent backup based on timestamp
- Downloads the file to the AWX controller
## 3. Apply
- Loads the downloaded configuration onto the device
- Uses replace: config for a full configuration restore
- Saves the configuration to NVRAM
## 4. Verify
- Confirms device accessibility after restore
- Validates key configuration elements (e.g., hostname)
- Flags restore_failed: true if verification does not meet expectations
## 5. Rollback
Executed only when verification fails:
- Reapplies the backup configuration
- Saves the rollback configuration
## 6. Cleanup
- Removes temporary files from the AWX controller

# 🚀 Example Playbook
---
- name: Restore Cisco IOS config from latest backup
  hosts: all
  gather_facts: no
  connection: network_cli

  vars:
    ansible_network_os: cisco.ios.ios

  roles:
    - role: ios_common/restore


This role requires no additional variables in the playbook.

AWX/AAP injects all necessary credentials securely.

# 🔐 Credential Handling
This role relies on AWX/AAP for secure credential injection.

The following variables must be provided by AWX and must not be stored in Git:

sftp_host: "<provided by AWX/AAP>"
sftp_username: "<provided by AWX/AAP>"
sftp_password: "<provided by AWX/AAP>"



# 🛠 Device Prerequisites (Bootstrap Configuration)
Before running the restore workflow, the target device must have a minimal configuration that allows Ansible to connect:

username <username> privilege 15 secret <password>
ip domain-name example.com
crypto key generate rsa modulus 2048
ip ssh version 2
aaa new-model
aaa authentication login default local
line vty 0 4
 login local
 transport input ssh

Additionally, the device must have:
- A reachable management IP
- A default gateway

# 🧪 Verification Logic
The restore is considered successful when:
- The device responds to commands
- The hostname or other key indicators match expected values
- No errors occur during configuration load
If verification fails, the workflow automatically triggers rollback.

# 🔁 Rollback Logic
Rollback re-applies the same backup file that was downloaded:
- Ensures the device returns to a known, validated configuration
- Prevents partial or incomplete restores
- Maintains operational consistency

# 📄 License
This project is licensed under the Eclipse Public License - v 2.0.

Refer to the LICENSE file for full details.

# 🤝 Contributing
Contributions and improvements are welcome.
Please follow the existing modular structure and commit standards when submitting changes.

## 👤 Author
**Bandar Shaik Dada Khalandar**  
Senior Network Engineer