**ISM-0487 Compliance Validation (Security Researcher Perspective)**

As a security researcher, validating compliance with **ISM-0487** involves assessing the configuration of **SSH connections that utilize logins without a passphrase (e.g., key-based authentication without a key passphrase)**. The control specifically mandates the disabling of certain functionalities in such scenarios to reduce the attack surface.

**ISM-0487 Reference:** "When using logins without a passphrase for SSH connections, the following are disabled:

- access from IP addresses that do not require access
- port forwarding
- agent credential forwarding
- X11 forwarding
- console access."

This control aims to mitigate the risk associated with compromised SSH keys that lack a passphrase, preventing an attacker from easily leveraging them for further lateral movement, data exfiltration, or unauthorized access.

Here are the TTPs you should employ to check and verify compliance with ISM-0487, including what to test and examine:

**I. Configuration Review (Primary Method)**

- **Tactic:** Secure Configuration Verification
- **Technique:** Examine SSH server configuration files (sshd_config) and client configurations to ensure mandated restrictions are in place.
- **Procedure:**
    1.  **Identify SSH Servers:** Determine all systems acting as SSH servers within the scope.
    2.  **Locate sshd_config:** On Linux/Unix-like systems, the primary SSH server configuration file is typically /etc/ssh/sshd_config.
    3.  **Identify Key-Based Authentication:** Look for configurations that enable key-based authentication, specifically where passphrases might not be enforced (e.g., PubkeyAuthentication yes).
    4.  **Verify Disabled Features:** Within sshd_config (or relevant client configurations if applicable), examine the settings for the following directives:
        - **AllowTcpForwarding no**: Disables port forwarding (local, remote, and dynamic).
        - **AllowAgentForwarding no**: Disables agent credential forwarding.
        - **X11Forwarding no**: Disables X11 forwarding.
        - **PermitRootLogin no** (or prohibit-password / without-password if root uses keys without passphrase): While not directly "console access" in the same way, it prevents direct root login via SSH, which is a common hardening step.
        - **PermitTTY no** (for specific users/groups that might use keys without passphrase): Disables pseudo-terminal allocation, which effectively prevents interactive console access. This is more granular.
        - **ListenAddress / AllowUsers / AllowGroups / DenyUsers / DenyGroups**: These control access from IP addresses. While not a direct "disable" for IP addresses, the absence of specific Allow directives or the presence of Deny directives for unauthorized IPs would indicate compliance.
- **Examples:**
    1.  **Test:** SSH into a target server (if authorized) and attempt to:
        - Establish a local port forward: ssh -L 8080:localhost:80 &lt;user&gt;@&lt;server&gt;
        - Establish an agent forward: ssh -A &lt;user&gt;@&lt;server&gt;
        - Establish an X11 forward: ssh -X &lt;user&gt;@&lt;server&gt;
        - Attempt to gain console access: ssh &lt;user&gt;@&lt;server&gt; (if PermitTTY no is set, you might not get a shell).
    2.  **Examine:**
        - Look for AllowTcpForwarding yes when it should be no.
        - Look for AllowAgentForwarding yes when it should be no.
        - Look for X11Forwarding yes when it should be no.
        - Check for PermitTTY yes for users with passphrase-less keys.
        - Verify that ListenAddress is configured only for necessary interfaces, and that AllowUsers/AllowGroups are tightly controlled, with no broad Allow rules that would permit access from unauthorized IPs.

**II. Network-Level Verification**

- **Tactic:** Network Access Control Assessment
- **Technique:** Verify that network-level controls (firewalls) restrict access to SSH services from unauthorized IP addresses.
- **Procedure:**
    1.  **Firewall Rule Review:** Examine firewall configurations (host-based like iptables, firewalld, Windows Firewall, or network-based firewalls) that protect SSH ports (default 22).
    2.  **Source IP Restrictions:** Look for rules that explicitly permit SSH access only from known, authorized source IP addresses or subnets.
    3.  **Default Deny:** Confirm that the default firewall policy for SSH is "deny all" and only explicitly allowed traffic is permitted.
- **Examples:**
    1.  **Tools:** iptables -L, firewall-cmd --list-all, netsh advfirewall firewall show rule name=all (Windows), review network firewall policies.
    2.  **Test:** From an unauthorized IP address, attempt to connect to the SSH server.
        - ssh &lt;user&gt;@&lt;server&gt;
        - nmap -p 22 &lt;server&gt; (to check if port is open from unauthorized location).
    3.  **Examine:** Look for ANY or 0.0.0.0/0 in source IP fields for SSH rules when more restrictive access is required.

**III. Active Directory/Identity Management Integration (If Applicable)**

- **Tactic:** Centralized Access Control
- **Technique:** If SSH access is integrated with Active Directory or another centralized identity management system, verify that access policies align with ISM-0487.
- **Procedure:**
    1.  **Review Integration:** Understand how SSH authentication (especially key-based) is managed. Is it directly on the host, or integrated with LDAP/AD?
    2.  **Policy Enforcement:** If AD groups control SSH access, verify that users/groups allowed to use passphrase-less keys are appropriately restricted from other functionalities (as per ISM-0487).
- **Examples:**
    1.  **Test:** Attempt to use a passphrase-less key from an account that is not in the authorized AD group for SSH access.
    2.  **Examine:** Review AD group memberships that grant SSH access and ensure they are tightly controlled and consistent with the principle of least privilege.

**IV. Auditing & Logging**

- **Tactic:** Detective Controls
- **Technique:** Verify that SSH activity, especially attempts to use disabled features, is logged and monitored.
- **Procedure:**
    1.  **SSH Log Configuration:** Ensure SSH server logging is enabled and configured for sufficient verbosity (e.g., LogLevel VERBOSE in sshd_config).
    2.  **Log Collection:** Confirm SSH logs are sent to a centralized logging system (e.g., SIEM like Elastic Stack).
    3.  **Alerting:** Verify that alerts are configured for suspicious SSH activity, such as:
        - Repeated failed login attempts.
        - Attempts to use disabled features (e.g., port forwarding attempts being logged as denied).
        - Logins from unusual source IPs.
- **Examples:**
    1.  **Test:** After attempting a port forward (which should be denied), check the SSH server logs (/var/log/auth.log on Linux) and the centralized logging system for entries indicating the denial.
    2.  **Examine:** Look for gaps in SSH logging, or evidence of successful use of features that should be disabled.

By systematically applying these TTPs, a security researcher can thoroughly assess an organization's compliance with ISM-0487, specifically focusing on the secure configuration of SSH connections that utilize passphrase-less authentication. Always ensure testing is performed within an authorized scope and all findings are meticulously documented.
