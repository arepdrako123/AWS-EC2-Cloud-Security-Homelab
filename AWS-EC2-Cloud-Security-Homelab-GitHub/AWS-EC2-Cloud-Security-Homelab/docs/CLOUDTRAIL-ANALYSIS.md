# AWS CloudTrail — `RunInstances` audit review

## Purpose

Validate that the lab instance creation generated an auditable AWS management event, then review its attributes without publishing the full event record.

## Observed fields (sanitized)

```json
{
  "eventSource": "ec2.amazonaws.com",
  "eventName": "RunInstances",
  "eventTime": "2026-10-26T22:37:33Z",
  "awsRegion": "ap-southeast-2",
  "eventCategory": "Management",
  "userIdentity": { "type": "AssumedRole", "sessionContext": { "attributes": { "mfaAuthenticated": "false" } } },
  "requestParameters": {
    "instanceType": "t3.micro",
    "monitoring": { "enabled": false },
    "metadataOptions": { "httpTokens": "required" },
    "blockDeviceMapping": { "items": [ { "ebs": { "volumeSize": 8, "volumeType": "gp3", "encrypted": false } } ] }
  },
  "responseElements": { "instancesSet": { "items": [ { "instanceState": { "name": "pending" } } ] } }
}
```

The record was made at **22:37:33 UTC on 26 October 2026**, corresponding to **06:37:33 Malaysia time on 27 October 2026**.

## Interpretation

- `RunInstances` is the EC2 API request used to launch an instance. The matching CloudTrail record is evidence of the management API action.
- `userIdentity.type: AssumedRole` means the request was attributed to an assumed AWS role, not directly to a named IAM user in this excerpt.
- `httpTokens: required` means IMDSv2 session tokens are required, a helpful EC2 metadata security control.
- `encrypted: false` means the root EBS volume was launched unencrypted: a remediation point for future iterations.
- `monitoring.enabled: false` means detailed monitoring was off; default/basic EC2 metrics still appeared in CloudWatch.
- `mfaAuthenticated: false` describes the **role-session context** field in this event, not conclusive proof of how the user authenticated to all parts of AWS. AWS account MFA should be reviewed separately.
- The response state `pending` was the initial state at launch, not its later `Running` state.

## Security incident example

If an unknown EC2 instance appears in an organization account, filter CloudTrail management events by `RunInstances` and verify the event time, identity context, Region, source IP (privately), and affected resource identifier. Correlate with authorized change records and additional logs before treating it as a security incident.

## Publication safeguards

The **raw CloudTrail JSON is not in this repository**, because it includes account identifiers, role ARNs, a temporary access-key identifier, a source IP address, and resource IDs. The screenshot in `screenshots/11-cloudtrail-runinstances-event.png` redacts selected identifiers. An access-key ID alone is **not** a secret key, but publishing live identifiers is unnecessary for this portfolio.
