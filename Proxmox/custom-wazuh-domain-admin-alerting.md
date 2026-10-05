# Custom Wazuh Detection and Email Alerting for Domain Admins

**Date:** October 5, 2026

## Objective

Extend the existing Active Directory monitoring lab so that privileged membership changes involving the **Domain Admins** group generate a custom high-severity Wazuh alert and an email notification.

## Detection Workflow

```text
Controlled AD membership change
        |
        v
Windows Security Event 4728 / 4729
        |
        v
Wazuh Windows event decoding
        |
        v
Built-in privileged group rule 60159
        |
        v
Custom child rule 100100 - Level 14
        |
        v
Postfix local relay
        |
        v
Authenticated Gmail SMTP relay
        |
        v
Email alert delivered
```

## Custom Rule

The custom rule was added to `/var/ossec/etc/rules/local_rules.xml` as a child of Wazuh rule `60159`.

```xml
<group name="windows,active_directory,privileged_group,">
  <rule id="100100" level="14">
    <if_sid>60159</if_sid>
    <field name="win.eventdata.targetUserName" type="pcre2">(?i)^Domain Admins$</field>
    <description>CRITICAL: Privileged Domain Admins membership changed.</description>
    <group>active_directory,privileged_group_change,</group>
  </rule>
</group>
```

The ruleset was syntax-tested with:

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

The Wazuh manager was restarted and confirmed `active (running)` before validation.

## Validation

A controlled account named `PrivTest` was used to generate repeatable Active Directory membership changes.

- Adding `PrivTest` to **Domain Admins** generated Windows Security Event ID `4728`.
- Removing `PrivTest` from **Domain Admins** generated Windows Security Event ID `4729`.
- Wazuh matched custom Rule ID `100100` at **Level 14**.
- The alert description was `CRITICAL: Privileged Domain Admins membership changed.`
- Event details identified the changed member, `Domain Admins` target group, administrative actor, domain, and domain controller.

## Email Alerting

Wazuh's local email path was configured through Postfix. Postfix was configured to use Gmail as an authenticated SMTP relay with a Google App Password rather than the normal Google account password.

The mail path was tested independently before enabling Wazuh email notifications.

Validated sequence:

1. Installed Postfix, `mailutils`, SASL modules, and CA certificates.
2. Configured Postfix relay host as `smtp.gmail.com:587` with TLS and SASL authentication.
3. Protected `/etc/postfix/sasl_passwd` and its database with root ownership and `600` permissions.
4. Sent a standalone `Wazuh Postfix Test` message and confirmed delivery.
5. Enabled Wazuh email notifications with `smtp_server` set to `localhost`.
6. Left the global Wazuh email threshold at Level `12`, allowing the custom Level `14` rule to qualify.
7. Triggered another controlled Domain Admins membership change.
8. Confirmed receipt of a Wazuh email showing **Alert level 14**, Rule `100100`, Event ID `4728`, and the custom critical description.

## Troubleshooting Performed

Initial Gmail relay attempts failed with:

```text
SASL authentication failed; Username and Password not accepted
```

The issue was isolated using `mailq`. The root cause was authentication: Gmail required a dedicated **Google App Password** with 2-Step Verification enabled. After replacing the regular account password with the App Password, Postfix successfully delivered queued mail.

A duplicate blank `relayhost` entry in `/etc/postfix/main.cf` was also identified and removed to keep the configuration clean.

## Security Practices

- Used a controlled test account and reversible membership changes.
- Used a custom child rule instead of modifying Wazuh's built-in rules.
- Validated rule syntax before restarting the Wazuh manager.
- Used a Google App Password instead of storing the normal Google account password.
- Restricted SMTP credential-file permissions to root only.
- Tested the mail relay independently before enabling Wazuh notifications.

## Skills Demonstrated

- Wazuh custom rule development
- Active Directory privileged-group monitoring
- Windows Security Event analysis (`4728`, `4729`)
- Detection severity tuning
- SIEM validation and troubleshooting
- Linux service administration
- Postfix SMTP relay configuration
- SASL/TLS authentication troubleshooting
- Secure credential-file permissions
- Alert notification workflow validation
- SOC-style detection engineering and documentation

## Result

The lab now provides an end-to-end privileged-access monitoring workflow: a Domain Admins membership change is captured by Windows, analyzed by Wazuh, escalated through a custom Level 14 rule, and delivered as an email alert for immediate review.
