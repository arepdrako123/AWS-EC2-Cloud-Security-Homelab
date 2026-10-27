# Security review and improvement roadmap

## Confirmed lab controls

- **SSH key access:** Windows PowerShell connected using a `.pem` private key; successful public-key authentication was observed.
- **Effective SSH settings:** `passwordauthentication no`; `pubkeyauthentication yes`; `permitrootlogin prohibit-password`.
- **AWS Security Group:** SSH (22/TCP) was scoped to `My IP` when configured; HTTP (80/TCP) was public to display the Nginx demo.
- **Ubuntu UFW:** Active after adding rules for 22/TCP and 80/TCP, with default `deny incoming`, default `allow outgoing`, and logging on `low`.
- **Post-change verification:** Successfully reconnected over SSH using a second PowerShell window after UFW was enabled.
- **CloudWatch:** CPU/network metrics available.
- **CloudTrail:** `RunInstances` management event available in Event history.

## Findings not yet remediated

| Priority | Observation | Recommended future work |
| --- | --- | --- |
| High before production | Demo website is HTTP-only | Use a domain, TLS/HTTPS, redirect HTTP to HTTPS, then consider port 443 security-group/UFW rules. |
| Medium | Root EBS volume was launched unencrypted | Enable EBS encryption by default for future encrypted instances/volumes; plan migration if retaining this one. |
| Medium | MFA had not been demonstrated and CloudTrail assumed-role field showed `false` | Verify account/identity-provider MFA; do not interpret the single CloudTrail field as definitive. |
| Medium | `PermitRootLogin prohibit-password` does not entirely disable root key-based SSH login | Consider `PermitRootLogin no` after validating a working non-root admin and backups. |
| Medium | UFW permits port 22 from any source at the host layer | Keep AWS Security Group SSH access tightly scoped to a known IP, or consider AWS Systems Manager Session Manager with appropriate roles. |
| Low | EC2 T3 CPU credits were set to unlimited | Monitor usage and promotional credit balance; avoid leaving unexpected high CPU workloads running. |
| Low | CloudWatch default metrics are not full security telemetry | For a later SOC-style phase, consider vetted host logs, alerting, and least-privilege IAM. Check pricing before enabling paid features. |

## Safe hardening practice

Avoid changing SSH and firewall settings blindly on a remote-only machine. Before editing access controls: record the existing rules; keep the active SSH session open; preserve the private key securely; test a second SSH connection after every change; maintain a recovery plan.

## Validation still needed

The screenshots show **post-UFW SSH access** and the **pre-UFW public website deployment**. The website was not separately verified in the provided evidence after UFW activation, so this repository does not claim that separate verification was completed.
