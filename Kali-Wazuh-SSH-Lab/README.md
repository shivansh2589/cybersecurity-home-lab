# Kali → Ubuntu SSH Detection and Hardening

**Hands-on home-lab work, validated October 8, 2026.** Controlled activity against owned VMs; no public or third-party systems were targeted.

## Objective

Trace a bounded SSH password test from Kali through Ubuntu's journal into Wazuh, identify the source and targeted account, validate a custom rule, apply an account-specific defense, and repeat the same test while preserving administrator access.

## Architecture

```text
Kali (sole adapter on internal vmbr1; no default route)
  → Ubuntu lab interface (vmbr1)
    → journald → Wazuh agent 4.14.8
      → Ubuntu management interface (vmbr0)
        → Wazuh manager 4.14.8 → Threat Hunting
```

The new bridge had no physical ports and no host IP. Kali had only its connected lab-subnet route; a route lookup for the management gateway returned “Network is unreachable.” Ubuntu retained its management connection for Wazuh and administrator access. Both IPv4 and IPv6 forwarding on Ubuntu were disabled. Ubuntu remained on the management LAN; the entire environment was not air-gapped.

The approximately 16 GiB Proxmox host could not comfortably run every VM. The unrelated Windows Server VM was stopped for this scenario. First, a dependency between the personal laptop's DNS and that server was identified and resolved using a verified gateway DNS resolver. The laptop was not domain joined. This did not alter the domain controller's DNS configuration.

## Verified results

| Stage | Observed evidence |
|---|---|
| Initial single-port scan | TCP 22 filtered; existing UFW policy allowed SSH from the management subnet only. |
| Scoped test allowance | Allowed only Kali's lab source to Ubuntu's lab address on the lab interface, TCP 22. Repeat scan showed open. |
| Baseline authentication | Three client password prompts; only **two server-confirmed failed-password events**, followed by a timeout. |
| Built-in detection | Two rule 5760 alerts at level 5; one related PAM rule 5503 alert at level 5. |
| Custom detection | Rule 100510 at level 6; positive, changed-source, and changed-account log tests; fresh live alert verified in manager JSON and dashboard. |
| Defense | Disabled password authentication for the dedicated sshlab account. Keyboard-interactive authentication was already disabled. |
| Repeat test | Same password-only SSH command rejected with “Permission denied (publickey).” No password prompt. Server recorded preauthentication connection closure. |
| Preservation | A new administrator SSH login succeeded. Existing email-monitoring rules 100500–100502 were preserved. |
| Cleanup | Temporary lab firewall allowance removed; UFW remained active with its original management-subnet SSH allowance. |

**Alert counts are not attempt counts.** PAM and SSH may describe the same authentication attempt. No successful compromise occurred.

## Setup and controlled test

Ubuntu initially had no Wazuh agent. The dashboard deployment command installed the DEB amd64 package matching manager version 4.14.8. Service status, manager agent listing, and dashboard all confirmed the enrolled agent was active. Journal monitoring was observed in the agent log.

A separate Ubuntu lab adapter was added; its lab address was temporary and would disappear after reboot. Kali used a separate NetworkManager manual-address profile without gateway or IPv6 and with autoconnect disabled. The existing profile was preserved. The lab profile needs manual activation after reboot.

Illustrative addresses below are documentation placeholders, **not runnable copies of the actual private addressing**. Replace them only with verified addresses of owned lab systems.

```bash
nmap -sT -Pn -n -p 22 --max-retries 1 --host-timeout 15s LAB_TARGET_IP -oN ssh-baseline-scan.txt
```

After the initial filtered result, the listener and UFW policy were inspected. A temporary allowance was scoped by interface, source, destination, and port; the original scan file was preserved and the allowed scan was saved separately.

A dedicated non-sudo account, sshlab, was created with private credentials. The server host fingerprint was obtained for comparison during the first SSH connection. A bounded password test was followed by single-attempt live validation tests; no wordlists, broad scans, or automated guessing campaigns were used.

```bash
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no -o NumberOfPasswordPrompts=1 -o ConnectTimeout=5 sshlab@LAB_TARGET_IP
```

Before hardening, one deliberately incorrect password was entered. After hardening, the same command was rejected before offering a password prompt.

## Investigation

Ubuntu authentication messages used the program name **sshd-session**. An initial journal query using only the sshd tag missed those events; a broader journal query recovered them. Wazuh's sshd decoder still decoded the newer process name correctly.

The source appeared in data.srcip and the target account in data.dstuser. The agent IP represented Ubuntu's management address; it was not the lab destination address used by Kali.

Queries used the enrolled agent ID plus the source or rule ID. The final rule 100510 event was checked in alerts.json before verifying it in Threat Hunting. A missing custom-rule search result initially reflected the event matching 5760, rather than proof that collection or indexing had failed.

## Custom rule

See [configs/ssh_lab_rules.xml](configs/ssh_lab_rules.xml). The example substitutes a documentation-only source address. Rule IDs were checked for conflicts before use.

The rule is a child of 5760 and matches the known lab source and dedicated account. It identifies **individual lab failures**; it is not a general brute-force correlation detector and does not infer malicious intent merely from an authentication failure.

The installed rule 5763 used frequency="8", timeframe="120", and same_source_ip. Its trigger threshold was not validated by this small test. No claim is made that this project validated repeated-failure correlation.

An original hostname condition matched in logtest but failed on a real event. Removing that condition resolved the mismatch in the next live test. The exact internal cause was not established. The final rule applies to matching source/account events across monitored endpoints; it is not constrained to a particular agent.

Validation:
1. Actual failure log → rule 100510, level 6.
2. Changed source → built-in rule 5760.
3. Changed account → built-in rule 5760.
4. wazuh-analysisd -t → exit code 0.
5. Manager restart → active.
6. Fresh real failure → rule 100510 in alerts.json and Threat Hunting.

The negative samples were simulated logtest inputs, not network attempts. The negative tests preceded removal of the hostname restriction; they were not rerun afterward. Live validation was completed for the revised rule.

## Defensive improvement

See [configs/00-sshlab-hardening.conf](configs/00-sshlab-hardening.conf).

```text
Match User sshlab
    PasswordAuthentication no
    KbdInteractiveAuthentication no
Match all
```

Configuration syntax was checked with sshd -t. Effective settings were checked with sshd -T -C for both the lab account and the administrator before reload. The current administrator session remained open, and a new administrator session succeeded after reload.

Public-key authentication remained enabled. **No authorized key was configured or successful key login demonstrated for sshlab.** This defense removed the password authentication path for the test account; it did not prove all possible access paths were blocked.

The post-change connection produced a preauthentication closure rather than a failed password. Consequently, a new failed-password rule 100510 alert is not expected for that connection. Absence of an alert alone was not used as evidence of prevention: client behavior, effective settings, and server logs were combined.

## Evidence timeline

UTC timestamps taken from observed server logs and alerts:

| Time | Event |
|---|---|
| 20:48:02 / 20:48:17 | Two baseline failed-password events. |
| 20:48:22 | Baseline timeout. |
| 23:00:15 | Fresh failure matched 5760 with the original custom hostname condition. |
| 23:16:36 | Fresh failure after removing hostname restriction. |
| 23:16:37.742 | Custom rule 100510 alert. |
| 23:22:15 | SSH reload with account hardening. |
| 23:23:45 | Repeat connection closed before authentication. |
| Approximately 23:26 | Fresh administrator login demonstrated; firewall allowance subsequently removed. |

## Published screenshot evidence

These are cropped, manually redacted copies of original screenshots. IP addresses and the personal terminal username were covered; timestamps, rule details, and observed behavior remain visible. No synthetic output was added.

### Custom Wazuh alert

![Wazuh custom SSH authentication failure alert, rule 100510 at level 6](evidence/01-custom-ssh-alert-redacted.png)

The dashboard shows the sshlab account, sshd decoder, rule 100510, level 6, and the alert timestamp. This confirms the real custom detection reached Threat Hunting.

### Password-only SSH test before and after hardening

![Kali password-only SSH attempts before and after account hardening](evidence/02-ssh-hardening-test-redacted.png)

The earlier attempts offered a password prompt and ended with `Permission denied (publickey,password)`. The final identical password-only command ended with `Permission denied (publickey)` without a password prompt. This demonstrates removal of the password authentication path; it does not demonstrate successful key login.

See [evidence/README.md](evidence/README.md) for evidence scope and timezone context.

## Lessons and limits

- Private addressing alone does not establish isolation; verify adapters, routes, bridge ports, and forwarding.
- Check resource dependencies before shutting down infrastructure VMs.
- Distinguish client prompts, server failures, and overlapping alerts.
- Test rules against both matching and nonmatching samples, then verify live behavior.
- Confirm effective SSH settings before reload and verify fresh administrator access afterward.
- A change in authentication methods changes which logs and alerts are expected.
- Ubuntu's bundled Ubuntu 22.04 SCA policy was skipped because the target ran a newer Ubuntu version. SSH monitoring was verified separately; no SCA compliance claim is made.
- This is hands-on home-lab experience, not enterprise production deployment.

## References

- [Wazuh custom rules](https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html)
- [Wazuh rule testing](https://documentation.wazuh.com/current/user-manual/ruleset/testing.html)
- [OpenSSH server configuration](https://man.openbsd.org/sshd_config)
- [Proxmox networking](https://pve.proxmox.com/wiki/Network_Configuration)
- [Nmap port states](https://nmap.org/book/man-port-scanning-basics.html)
