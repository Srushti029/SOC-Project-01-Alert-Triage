# SOC-Project-01-Alert-Triage
SOC investigation of suspicious Windows authentication activity using synthetic security logs.

The investigation focuses on identifying abnormal authentication behaviour, correlating source IP addresses with user accounts, reconstructing the event timeline, classifying the alert, and recommending an appropriate response.

## Objective

- Analyse Windows authentication events
- Identify abnormal login patterns
- Correlate source IP addresses and user accounts
- Reconstruct the incident timeline
- Classify the security alert
- Assess severity
- Recommend an appropriate response

## Environment

- Synthetic Windows security/authentication logs
- Simulated internal network
- Windows Security Event IDs: 4624, 4625, 4634, 4672

## Dataset

The investigation was performed using a small synthetic Windows authentication log dataset created specifically for this security exercise.

The dataset simulates authentication failures, successful logons, multiple user accounts, and source-IP activity.

No real organizational logs, credentials, or sensitive information were used.

## Investigation Process

The raw events were reviewed chronologically and grouped by source IP and account.

The investigation focused on:

1. Repeated authentication failures
2. Successful authentication following failed attempts
3. Activity involving multiple accounts
4. Source-IP correlation
5. Differences between authentication patterns

## Key Findings

Source IP `10.10.20.15` generated repeated failed authentication attempts against the `srushti` account.

A total of **seven failed logon attempts** were observed between `08:41:12` and `08:42:03`, followed by a successful logon at `08:42:17`.

This created a **14-second gap between the final failed attempt and the successful authentication**.

The same source IP also generated failed authentication attempts against the `admin` account, indicating activity involving multiple accounts.

By comparison, `10.10.20.44` showed successful logons and logoffs for `srushti` and a SYSTEM special-privilege event, with no failed authentication attempts in the provided dataset.

## Analysis

The sequence of repeated failed authentication attempts followed by a successful logon is consistent with **password-guessing or brute-force activity**.

The presence of authentication failures against multiple accounts from the same source IP increases the level of concern and makes `10.10.20.15` the primary source requiring further investigation.

The logs alone do not prove attacker identity or definitively establish account compromise, so additional evidence would be required.

## Final Assessment

| Field | Assessment |
|---|---|
| Classification | **True Positive** |
| Severity | **High** |
| Primary Source IP | `10.10.20.15` |
| Affected Accounts | `srushti`, `admin` |
| Likely Activity | Password guessing / brute-force |
| Confidence | High |

## Recommended Response

- Validate whether `10.10.20.15` is an authorised internal host.
- Investigate the `srushti` account for possible unauthorised access.
- Review additional authentication and endpoint telemetry around the suspicious timeframe.
- Investigate the `admin` account for additional activity.
- Consider temporary source restriction or host isolation if further evidence confirms malicious activity.
- Reset affected credentials if compromise is confirmed.

## Evidence

### Authentication Log Analysis

![Authentication Log Analysis](screenshots/01-investigation-notes.png)

### Source IP Correlation

![Source IP Correlation](screenshots/02-ip-correlation.png)

### Final Assessment

![Final Assessment](screenshots/03-final-verdict.png)

## Limitations

This project uses a small synthetic dataset and does not include complete endpoint telemetry, DNS data, network-flow data, asset ownership information, or full Windows event details.

The investigation therefore identifies suspicious authentication behaviour but does not independently prove attacker identity or account compromise.

## Skills Demonstrated

- Authentication log analysis
- Timeline reconstruction
- Source-IP correlation
- Account correlation
- SOC alert triage
- Severity assessment
- Incident-response recommendations
- Security documentation

## Disclaimer

This project was conducted using synthetic data in a controlled learning environment.

No real credentials, organisational logs, production systems, or unauthorised targets were used.
