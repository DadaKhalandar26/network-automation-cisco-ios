# IOS Standard Configuration Role (roles/config)

Description

This Ansible role is designed to enforce a Standard Baseline Configuration across Cisco IOS network devices. It ensures that all devices adhere to corporate compliance and security standards by pushing consistent configurations for management, security, and administrative settings.

It is strictly focused on Global Configuration (Day 0/Day 1 settings) rather than interface-specific routing logic.

# Capabilities
This role handles the configuration of the following standard services:

- AAA (Authentication, Authorization, Accounting): Defines method lists for login and command authorization.
- TACACS+ / RADIUS: Configures remote server groups, keys, and source interfaces.
- NTP (Network Time Protocol): Sets time servers, timezones, and source interfaces to ensure accurate logging.
- SNMP (Simple Network Management Protocol): Configures read/write communities, trap receivers, and location/contact info.
- Syslog / Logging: Sets logging buffering, console levels, and remote syslog servers.
- Line VTY / Console: Secures remote access (SSH versions, timeouts, transport input, access lists).
- Banner: Sets standard MOTD (Message of the Day) and Login banners for legal compliance.
- Domain & DNS: Configures domain name, name servers, and lookup settings.
- User Management: Manages local fallback users and privilege levels.
- Service Hardening: Disables unused services (like packet assembly or HTTP server) and enables password encryption.

# Role Variables
Variables should be defined in your group variables or host variables files. The role supports settings for:

- AAA & Security: Enabling new model and defining authentication groups.
- TACACS+ Configuration: Defining hosts, keys, and timeouts.
- NTP Settings: Defining source interfaces and server IP addresses.
- SNMP: Defining location, contact email, and community strings (Read-Only and Read-Write).
- General Hardening: Setting the banner message and domain name.

# Notes
- Idempotency: This role ensures configurations are only changed if they differ from the desired state.
- Order of Operations: AAA is configured before VTY lines to ensure you do not lock yourself out during the playbook run.

# Author: Bandar Shaik Dada Khalandar
