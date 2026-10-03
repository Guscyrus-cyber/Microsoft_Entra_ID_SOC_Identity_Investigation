**Microsoft Entra ID SOC Interface & Identity Investigation Basics**

Introduction

Microsoft Entra ID is Microsoft's cloud-based identity and access management service used to manage users, authentication, applications, devices, and access to organizational resources. From a Security Operations Center (SOC) perspective, Microsoft Entra ID is an important source of identity-related security information because many attacks begin with attempts to compromise user accounts and credentials.

For SOC Tier 1 and Tier 2 analysts, Microsoft Entra ID provides valuable evidence for investigating activities such as failed authentication attempts, suspicious successful logins, brute-force attacks, password spraying, unusual sign-in locations, multifactor authentication events, account modifications, and unauthorized privilege changes.

This lab introduces the Microsoft Entra ID interface specifically from a SOC analyst perspective. The purpose is not to perform general Entra ID administration, but to identify and understand the areas of the platform that are most relevant to security monitoring, alert triage, and incident investigation.

The lab focuses primarily on Users, Sign-in logs, Audit logs, authentication information, IP addresses, applications, devices, locations, timestamps, and event status information. These data points can help an analyst reconstruct authentication activity and determine whether an event represents normal user behavior or potentially malicious activity.

A typical identity investigation may begin with a simple authentication event:

User → Authentication Attempt → Microsoft Entra ID → Sign-in Log

The analyst can then examine information such as:

User Identity → Time → Source IP → Location → Device → Application → Authentication Method → Result

Individually, these fields provide basic information about an authentication event. When correlated across multiple events, however, they can reveal suspicious patterns.

For example:

Repeated Failed Sign-ins

↓

Successful Sign-in

↓

New Source IP

↓

Unexpected Location or Device

↓

Possible Account Compromise

Microsoft Entra ID also maintains Audit logs, which provide evidence of changes made within the identity environment. These records can help analysts determine whether accounts, groups, roles, or other identity-related objects were modified following suspicious authentication activity.

This becomes particularly important for Tier 2 investigations, where an analyst may need to determine not only whether an account was compromised, but also what the account did after the compromise occurred.

Lab Objectives

By completing this lab, the analyst will learn how to:

- Navigate the Microsoft Entra admin center from a SOC perspective.

- Locate and examine users and identity information.

- Access Microsoft Entra ID Sign-in logs.

- Identify successful and failed authentication events.

- Examine source IP addresses and sign-in locations.

- Review application, device, browser, and authentication information when available.

- Access and interpret Microsoft Entra ID Audit logs.

- Identify important fields used during identity investigations.

- Understand the difference between authentication activity and administrative/audit activity.

- Establish the foundation required for later Microsoft Entra ID attack-investigation labs.

SOC Relevance

This introductory lab establishes the foundation for subsequent investigations involving:

Brute Force → Password Spraying → Suspicious Successful Sign-in → MFA Investigation → Privilege Changes → Account Compromise

The same identity information can later be integrated with Microsoft Sentinel, where SOC analysts can use Kusto Query Language (KQL), analytics rules, alerts, and incidents to detect and investigate suspicious Entra ID activity at a larger scale.

Therefore, this lab serves as the starting point for understanding the relationship between:

Identity → Authentication → Logs → Detection → Investigation → Incident Response

No external dataset is required for this introductory lab. The Microsoft Entra ID environment and the identity/security information available within the tenant will be used to become familiar with the SOC investigation workflow.

Microsoft Entra ID Lab 1 — SOC Interface & Identity Investigation Basics

Step 1 — Opening Microsoft Entra ID

From the Azure portal, I open Microsoft Entra ID from the main services menu.

This provides access to the tenant's identity and authentication management environment. (Image 1)\
\
\
\
\
\
\

Step 2 — Opening Users

Navigating to:

Microsoft Entra ID → Users → All users

This displays all user identities currently available in the tenant. (Image 2)

Step 3 — Review the User Identity

I Select the user account and I open the Overview page.

Review basic identity information such as:

- User principal name

- Object ID

- Account status

- User type

- Group memberships

- Applications

- Assigned roles

- Assigned licenses

This information helps establish the identity being investigated. (Image 3)

\
\
\
Step 4 — Open Sign-in Logs

I Select Sign-in logs from the user's left-side menu.

Sign-in logs provide authentication activity such as:

- Date and time

- Application

- Sign-in status

- Error code

- Source IP address

- Geographic location

These logs are one of the primary sources used for identity-related SOC investigations.\
\
Date \| Request ID \| User \| Application \| Status \| Sign-in error code \| IP address \| Location

This event shows:

User: Cyrus Mirzaei\
Application: Azure Portal\
Status: Success\
Error code: 0\
IP: 108.28.79.19\
Location: Lorton, Virginia, US (Image 4)

\
\
\
Step 5 — Open a Sign-in Event

I Select one successful authentication event to open Activity Details: Sign-ins.

I Review the Basic info section, including:

- User

- Date and time

- Status

- Authentication requirement

- Request ID

- Correlation ID

<!-- -->

- This provides detailed information about an individual authentication event.\
  \
  Date: 8/14/2026, 8:44:25 PM

- Status: Success

- User: Cyrus Mirzaei

- Authentication requirement: Single-factor authentication

- Additional details: “MFA requirement satisfied by claim in the token”

- Request ID / Correlation ID: identifiers we could use when correlating or troubleshooting this authentication event. (Image 5)\
  \
  \
  \
  \
  Step 6 — Review Sign-in Location

I Open the Location tab within the sign-in event.

Reviewing:

- Source IP address

- Geographic location

- Autonomous System Number

- Global Secure Access status

Location information can help identify unusual or unexpected authentication activity.\
\
Location: Lorton, Virginia, US\
IP address: 108.28.79.19\
Autonomous System Number (ASN): 701\
Through Global Secure Access: No

From a SOC perspective, the important lesson is that location alone does not prove whether a login is legitimate or malicious. A Tier 1 analyst would compare this event with the user's normal sign-in history.

For example, later I might encounter:

Normal: Virginia → 108.28.79.19\
Suspicious: another country → new IP address

That difference could become an important indicator during account-compromise triage.\
(Image 6)\
\
\
\
\
\
\
\
Step 7 — Review Device Information

I Open the Device info tab.

I Review available information such as:

- Browser

- Operating system

- Device ID

- Managed status

- Compliance status

- Join type

Device information can help determine whether the authentication originated from a familiar or unusual endpoint. (Image 7)\
\
\
\
\
\
\
\
\
\
\
\
Step 8 — Review Authentication Details

I Open the Authentication Details tab.

I Review how the authentication requirement was satisfied and whether the authentication succeeded.

This information helps determine whether credentials, MFA, or a previously established authentication session was used. (Image 8)\
\
\
\
\
\
\
Step 9 — Review Conditional Access

I Open the Conditional Access tab.

I Review the policy evaluated during the sign-in and its result.

In this lab, Security defaults were evaluated successfully.

Conditional Access information helps determine which identity security controls were applied during authentication. (Image 9)\
\
\
\
\
\
Step 10 — Review Audit Logs

I Close the sign-in event and I select Audit logs from the user's menu.

Audit logs are used to investigate directory and administrative changes such as:

- User modifications

- Password-related actions

- Role changes

- Group changes

- Authentication method changes

In this lab, no audit events were found within the selected seven-day period. (Image 10)\
\
\
\
\
\
\
\
Step 11 — Review Assigned Roles

I Select Assigned roles from the user's menu.

The account in this lab was assigned the:

Global Administrator

role.

Privileged accounts are especially important during SOC investigations because compromise of an administrative identity can result in significantly greater impact. (Image 11)\
\
\
\
\
\
\
Step 12 — Review Authentication Methods

I Select Authentication methods from the user's menu.

This area allows me to analysts and review registered authentication methods and identify potentially suspicious changes to MFA or other authentication mechanisms.

The lab also identified available incident-response actions such as:

- Reset password

- Require MFA re-registration

- Revoke sessions

No changes were made during this introductory lab. (Image 12)\
\
\
\
\
\
\
\
Lab Completion

The lab established a basic Microsoft Entra ID SOC investigation workflow:

User Identity

↓

Sign-in Logs

↓

Authentication Event

↓

Location

↓

Device Information

↓

Authentication Details

↓

Conditional Access

↓

Audit Logs

↓

Assigned Roles

↓

Authentication Methods

This workflow provides the foundation for later Microsoft Entra ID SOC investigations involving failed sign-ins, brute-force attacks, password spraying, suspicious successful authentication, MFA abuse, privilege escalation, and account compromise.
