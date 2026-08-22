# Cybersecurity-Microsoft-Scenario-Implementations
Scenario-based Microsoft identity and endpoint security implementations completed in authorized SC-900 and SC-200 labs.

# 1. Scenario Title: New Employee Onboarding with Microsoft Entra ID

**2. Executive Summary:** 
Implemented secure, group-based onboarding and least-privilege access for a new Cybersecurity Analyst. Built role-based access control (RBAC), provisioned Microsoft 365 licensing via group inheritance, and enforced Multi-Factor Authentication (MFA) via Conditional Access.

**3. Business Requirement:** 
Provision Microsoft 365 services, internal collaboration access, and read-only security visibility without granting permanent administrative permissions[cite: 3].

**4. Security Risks Addressed:** 
Mitigated the risks of unauthorized access and excessive privilege. Prevented administrator lockout by protecting break-glass emergency access.

**5. Lab Environment & Assumptions:** 
Executed within an authorized SC-900 lab tenant using fictional identities and an available Microsoft 365 E5 license[cite: 1, 3].

**6. Scope & Safety Boundaries:** 
Applied Conditional Access strictly to the `SG-CA-MFA-Required` group, explicitly excluding an emergency access account[cite: 3].

**7. Implementation Plan:** 
1. Provision the user account with enforced first-time password rotation.
2. Construct role-assignable and standard security groups.
3. Configure group-based licensing for the Microsoft 365 E5 subscription.
4. Establish a Conditional Access policy enforcing MFA.
5. Assign least-privilege Entra security roles via role-assignable groups.

**8. Configuration Decisions & Justification:** 
Utilized group-based licensing instead of direct assignment to ensure scalable access lifecycle management[cite: 3].

**9. Validation Plan:** 
Conduct positive and negative tests confirming effective access and denied administrative actions[cite: 1, 3].

**10. Expected vs. Actual Results:**
| Validation | Expected Result | Actual Result | Status |
| :--- | :--- | :--- | :--- |
| User Provisioning | Account active with required attributes | User created with correct job/department metadata | Pass |
| MFA & CA | Daniel is forced to register for MFA[cite: 3] | Password reset and MFA enforced | Pass |
| Read-only Role | Daniel can view security dashboards but cannot modify settings[cite: 3] | Security Reader access confirmed; user creation denied | Pass |
| Licensing | Inherited M365 E5 via security group | Group assignment active; direct assignment locked | Pass |

**11. Evidence Index:** 
* `ID-S1-01-user-attributes.png`: Proves Daniel's account creation with correct attributes[cite: 1, 3].
* `ID-S1-02-group-memberships.png`: Proves Daniel is assigned to all four required security, licensing, and M365 groups[cite: 3].
* `ID-S1-03-group-licence-assignment.png`: Configuration view of the Microsoft 365 E5 license assigned to `SG-LIC-M365-E5-SOC`[cite: 3].
* `ID-S1-04-conditional-access-policy.png`: Configuration of `CA-Require-MFA-SOC-Users` showing target groups, break-glass exclusion, and Report-only state[cite: 3].
* `ID-S1-05-role-assignment-least-privilege.png`: Proof of the `Security Reader` role assigned to `SG-Security-ReadOnly-Access`[cite: 3].
* `ID-S1-06-password-reset-validation.png`: Positive sign-in test showing forced password rotation on first authentication[cite: 3].
* `ID-S1-07-user-productivity-access.png`: User session proving successful access to provisioned productivity services[cite: 3].
* `ID-S1-08-licensing-troubleshooting.png`: Administrative log showing group-based license inheritance lock in Microsoft 365 Admin Center[cite: 1, 3].
* `ID-S1-09-mfa-challenge.png`: Positive test proving the Conditional Access policy successfully enforces an MFA challenge during Daniel's sign-in[cite: 1, 3].
* `ID-S1-10-denied-role-admin.png`: Negative test proving Daniel cannot modify administrative role assignments[cite: 1, 3].
* `ID-S1-11-denied-user-creation.png`: Negative test verifying least privilege; Daniel is explicitly blocked from creating new directory users[cite: 1, 3].

**12. Problems Encountered:** 
The Microsoft 365 Admin Center successfully applied the group-based license, but a lab portal limitation prevented the removal of the initial direct assignment tag.

**13. Security Outcome:** 
Established a verified least-privilege baseline for a standard operations user.

**14. Limitations:** 
Group-based licensing management has transitioned to the Microsoft 365 Admin Center, requiring cross-portal validation.

**15. Production Recommendations:** 
Transition the Conditional Access policy from report-only to enforcement mode after monitoring logs. 

**16. Lessons Learned:** 
Group-based routing is highly effective for scaling permissions, and isolating emergency access from Conditional Access is essential before enforcement.
