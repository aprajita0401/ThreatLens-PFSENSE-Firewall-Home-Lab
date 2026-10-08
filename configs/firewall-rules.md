# ThreatLens: Firewall Rules

This document is a **rule documentation template** for the ThreatLens
pfSense lab. Fill in the values from your actual configuration before
publishing. It is not a pfSense configuration export, and it does not
prove that a rule was tested successfully.

## Safety and management access

-   Keep pfSense management access on a trusted, restricted interface or
    management network.
-   Do not disable the firewall as a routine way to access the web
    interface.
-   Do not expose the pfSense web interface to the WAN.
-   Generate test traffic only in an isolated lab against systems you
    own or are authorized to test.
-   Confirm interface assignments and the traffic path before applying
    rules.

## Rule inventory

Document each relevant rule in the table below. The project PDF
describes an initial rule to permit test traffic and a later rule
intended to block traffic from Kali to Ubuntu. Verify the actual
interface and packet path in your own lab; do not assume a rule belongs
on WAN simply because an example says so.

 | Rule ID | Interface  | Action | Protocol   | Source                             | Destination                    | Logging                                   | Purpose / Status                                       |
| ------- | ---------- | ------ | ---------- | ---------------------------------- | ------------------------------ | ----------------------------------------- | ------------------------------------------------------ |
| FW-01   | LAN | Pass   | TCP | [trusted management subnet] | [pfSense management address] | Enabled if supported by the selected test| Restricted management access, if configured            |
| FW-02   | WAN | Pass   | TCP | Kali IP                 | Ubuntu WAN IP            |Enabled if supported by the selected test| Temporary lab allowance, only if required for the test |
| FW-03   | WAN | Block  | ICMP (any) | Kali IP                 | Ubuntu WAN IP              | Enabled if supported by the selected test | Block the test traffic                                 |

**Important:** These are documentation placeholders, not instructions to
copy blindly. Replace every `[verify]` value with the rule actually
present in your lab. Remove rows for rules you did not configure.

## Rule order and validation

pfSense evaluates rules according to interface and rule ordering.
Confirm that the blocking rule is on the interface where the relevant
traffic is evaluated and is ordered so that an earlier matching pass
rule does not allow the traffic first.

For the block-rule test, record:

1.  The test host and target addresses used.
2.  The interface where the rule is applied.
3.  The selected protocol and any port criteria.
4.  Whether logging is enabled.
5.  A screenshot of the actual rule.
6.  Matching firewall log entries, if generated.
7.  Packet-capture observations before and after applying the rule.

Do not claim that the attack was mitigated solely because the rule
exists. Correlate the rule, log entries, and packet-capture
observations.

## Test record

  Field             Record
  ----------------- -----------------------------------------------
  Test ID           `[e.g., TL-FW-01]`
  Date              `[YYYY-MM-DD]`
  Source host       `[Kali address / lab identifier]`
  Target host       `[Ubuntu address / lab identifier]`
  Rule tested       `[rule ID and description]`
  Expected result   `[what should happen]`
  Observed result   `[what actually happened]`
  Evidence          `[relative path to screenshots/log excerpts]`
  Status            `[Pass / Fail / Inconclusive]`

## Redaction before publishing

Before committing screenshots or configuration excerpts, redact
credentials, public IP addresses where sensitive, unique device
identifiers, and personal network details. Never publish passwords or
secrets.
