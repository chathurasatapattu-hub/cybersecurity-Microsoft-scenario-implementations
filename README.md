# cybersecurity-Microsoft-scenario-implementations
Scenario-based Microsoft identity and endpoint security implementations completed in authorized SC-900 and SC-200 labs.

# Scenario 1: New Employee Onboarding with Microsoft Entra ID

## 1. Scenario Title
**New Employee Identity Lifecycle, Group-Based Access Control, and Conditional Access Implementation**

---

## 2. Executive Summary
Designed and implemented an automated, group-based identity onboarding workflow in Microsoft Entra ID for a new Cybersecurity Analyst (Daniel Silva) joining the Security Operations team. Built role-based access control (RBAC), provisioned Microsoft 365 licensing via group inheritance, enforced Multi-Factor Authentication (MFA) via Conditional Access in Report-only mode with emergency access exclusions, and validated least-privilege administrative boundaries.

---

## 3. Business Requirement
Contoso Professional Services requires a standardized, scalable onboarding process for a new analyst in the Security Operations team. Requirements include:
* Fictional identity creation with accurate organizational metadata.
* Group-managed license allocation (avoiding direct user assignment).
* Mandatory MFA registration and sign-in policy protection.
* Read-only visibility into security telemetry without configuration change privileges.
* Segregated collaboration and internal sharing permissions.

---

## 4. Security Risks Addressed
* **Excessive Privilege Accumulation:** Prevented privilege creep by enforcing the Security Reader role rather than broad administrative roles.
* **Administrator Lockout:** Protected break-glass emergency administrative access by explicitly excluding it from Conditional Access scopes.
* **Credential Compromise:** Enforced mandatory password rotation at initial login and configured policy-driven MFA requirements.
* **Orphaned Direct Entitlements:** Eliminated unmanaged direct license and role assignments by leveraging group-based inheritance.

---

## 5. Lab Environment and Licensing Assumptions
* **Tenant:** Authorized SC-900 / Microsoft 365 lab environment.
* **Identities:** Fictional identity (`daniel.silva@yourdomain.com`) with zero production dependencies.
* **Licensing:** Microsoft 365 E5 suite available for assignment.
* **Administrative Context:** Executed using authorized administrative credentials.

---

## 6. Scope and Safety Boundaries
* **Target Scope:** Limited strictly to Daniel Silva and designated pilot security groups.
* **Safety Controls:** Conditional Access deployed in `Report-only` mode prior to production enforcement.
* **Break-Glass Protection:** Emergency access account excluded from the Conditional Access policy scope.

---

## 7. Implementation Plan
1. Provision the user account with enforced first-time password rotation.
2. Construct role-assignable and standard security groups.
3. Configure group-based licensing for the Microsoft 365 E5 subscription.
4. Establish a Conditional Access policy enforcing MFA.
5. Assign least-privilege Entra security roles via role-assignable groups.
6. Execute positive sign-in verification and capture troubleshooting telemetry.

---

## 8. Configuration Decisions and Justification

### Access Architecture Mapping
| Group Name | Group Type | Entitlement / Policy Target | Resource / Scope |
| :--- | :--- | :--- | :--- |
| `SG-LIC-M365-E5-SOC` | Security | Microsoft 365 E5 License | Exchange, SharePoint, Teams, Defender XDR |
| `SG-CA-MFA-Required` | Security | MFA Enforcement Policy Target | All Cloud Applications |
| `M365-Security-Operations` | Microsoft 365 | Site Collaboration Member | Security Operations SharePoint/Teams |
| `SG-Security-ReadOnly-Access` | Role-Assignable | Security Reader Role | Tenant Security & Compliance Portals |

* **Group-Based Governance:** Utilizing groups for licensing and roles ensures that identity offboarding requires only removing group memberships, automatically revoking licenses and permissions simultaneously.
* **Report-Only Policy Mode:** Configured `CA-Require-MFA-SOC-Users` in report-only state to evaluate policy impact across sign-in logs without causing operational disruption.

---

## 9. Validation Plan
* **Positive Tests:** Verify user creation, group inheritance, initial sign-in password change prompt, and application access.
* **Negative Tests:** Verify emergency access policy exclusion, direct assignment prevention, and administrative permission denial.

---

## 10. Expected-versus-Actual Results

| Test Area | Expected Outcome | Actual Outcome | Status |
| :--- | :--- | :--- | :--- |
| **User Provisioning** | Account active with required attributes | User created with correct job/department metadata | **Pass** |
| **Group Membership** | User added to all 4 functional groups | All memberships confirmed in Entra admin center | **Pass** |
| **Licensing** | Inherited M365 E5 via security group | Group assignment active; direct assignment locked | **Pass** |
| **Conditional Access** | Policy targets group, excludes break-glass | Verified 1 group included, 1 account excluded | **Pass** |
| **Least Privilege** | Read-only security role assigned | Security Reader assigned via role-assignable group | **Pass** |
| **Authentication** | User prompted to change password | Password reset dialog enforced on initial login | **Pass** |

---

## 11. Evidence Index

* `ID-S1-01-user-attributes.png`: Sanitized Entra ID user overview proving account creation, job title (`Cybersecurity Analyst`), and department (`Security Operations`).
* `ID-S1-02-group-memberships.png`: Proof of Daniel Silva's membership across all four operational and licensing groups.
* `ID-S1-03-group-licence-assignment.png`: Configuration view of the Microsoft 365 E5 license assigned to `SG-LIC-M365-E5-SOC`.
* `ID-S1-04-conditional-access-policy.png`: Configuration of `CA-Require-MFA-SOC-Users` showing target groups, break-glass exclusion, and Report-only state.
* `ID-S1-05-role-assignment-least-privilege.png`: Proof of the `Security Reader` role assigned to `SG-Security-ReadOnly-Access`.
* `ID-S1-06-password-reset-validation.png`: Positive sign-in test showing forced password rotation on first authentication.
* `ID-S1-07-user-productivity-access.png`: User session proving successful access to provisioned productivity services.
* `ID-S1-08-licensing-troubleshooting.png`: Administrative log showing group-based license inheritance lock in Microsoft 365 Admin Center.

---

## 12. Problems Encountered and Troubleshooting
* **Issue:** The Microsoft 365 Admin Center displayed a configuration warning stating licenses could not be modified directly because they were inherited from a group membership.
* **Root-Cause Analysis:** The tenant interface enforces group-based licensing governance by restricting manual direct license edits once group inheritance is established.
* **Resolution:** Verified that group licensing on `SG-LIC-M365-E5-SOC` was successfully pushing all required services to Daniel Silva, documenting the administrative lock as expected behavior.

---

## 13. Security Outcome
Established a verified, auditable identity baseline for new operations personnel. Authentication security is hardened through planned MFA policies, while operational risks are controlled via least-privilege read-only role assignments.

---

## 14. Limitations
* Group-based licensing management has transitioned to the Microsoft 365 Admin Center, requiring cross-portal validation.
* Conditional Access was maintained in Report-Only mode per lab safety protocols.

---

## 15. Production Recommendations
1. Transition `CA-Require-MFA-SOC-Users` from `Report-only` to `On` following a 7-day log review.
2. Implement Entra ID Access Packages and Access Reviews to automate group lifecycle audits.
3. Configure Named Locations in Conditional Access to enforce geographic restrictions.

---

## 16. Lessons Learned
* **Defense-in-Depth Identity Design:** Group-based entitlement models drastically reduce manual administrative errors during employee onboarding and offboarding.
* **Administrative Safeguards:** Explicitly excluding emergency break-glass accounts from Conditional Access policies is essential before enabling enforcement.
