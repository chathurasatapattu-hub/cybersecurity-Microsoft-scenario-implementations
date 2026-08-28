# Scenario 2: Controlled Privileged Access and SharePoint Administration

## 1. Scenario Title
**Implementing Just-In-Time (JIT) Administration, Secure Collaboration, and Offboarding Data Recovery**

---

## 2. Executive Summary
Designed and implemented a least-privilege administrative model using Privileged Identity Management (PIM) to replace standing access with Just-In-Time (JIT) elevation. Configured a secure SharePoint collaboration environment utilizing group-based access control, executed a secure data recovery operation for a departed employee, and enforced strict Conditional Access policies for all tenant administrators.

---

## 3. Business Requirement
Contoso requires temporary, auditable administrative access for a Support Engineer to build a project site (Project Phoenix). The site requires granular, group-based permissions and a highly restricted executive document library. Additionally, the engineer must securely recover a former employee's OneDrive data without retaining permanent access.

---

## 4. Security Risks Addressed
* **Standing Administrative Privilege:** Mitigated the risk of compromised admin accounts by converting standing access to eligible, time-bound PIM roles.
* **Data Exfiltration:** Secured confidential executive documents by breaking site-wide permission inheritance.
* **Unauthorized Data Retention:** Ensured offboarded user data was recovered via temporary, explicit delegate links rather than broad administrative sweeping, followed by immediate access revocation.

---

## 5. Lab Environment and Assumptions
* **Tenant:** Authorized SC-900 / Microsoft 365 lab environment.
* **Identities:** Executed using standard lab users (`Sarah Fernando`, `Alex Wilber`, `Cameron White`). 

---

## 6. Scope and Safety Boundaries
* **Target Scope:** PIM applied strictly to the `SharePoint Administrator` role. Conditional Access scoped to directory roles.
* **Safety Controls:** Emergency access account explicitly excluded from the global administrative Conditional Access policy to prevent tenant lockouts.

---

## 7. Implementation Plan
1. Configure `SharePoint Administrator` role for JIT activation via PIM (2-hour limit, MFA, and justification required).
2. Validate standard user restrictions prior to PIM activation.
3. Build the "Project Phoenix" SharePoint site and map custom Entra security groups to Site Owners, Members, and Visitors.
4. Establish a restricted document library with broken permission inheritance.
5. Disable an offboarded user account, generate a temporary OneDrive recovery link, and revoke access post-recovery.
6. Deploy `CA-Admin-Strong-Authentication` policy for administrative roles.

---

## 8. Configuration Decisions and Justification
* **PIM Group Assignment:** Assigned PIM eligibility to a dedicated security group (`SG-PIM-SharePoint-Admins`) rather than the user directly, ensuring scalable role management.
* **Report-Only Policy Mode:** Deployed the admin Conditional Access policy in Report-only mode to validate the impact on service accounts before enforcing block actions.

---

## 9. Validation Plan
* **Positive Tests:** Verify successful PIM activation log and successful access to SharePoint Admin Center. Verify temporary OneDrive delegate link generation.
* **Negative Tests:** Verify inability to perform unauthorized global tasks (e.g., User Administrator actions) while elevated as a SharePoint Administrator.

---

## 10. Expected-versus-Actual Results

| Test Area | Expected Outcome | Actual Outcome | Status |
| :--- | :--- | :--- | :--- |
| **PIM Activation** | User must provide justification to elevate | Role activated successfully with logged justification | **Pass** |
| **Least Privilege** | PIM user cannot create Entra accounts | "New User" creation explicitly blocked | **Pass** |
| **Site Security** | Standard members cannot view executive library | Inheritance broken; Members removed from ACL | **Pass** |
| **Offboarding Recovery** | Data recovered via temporary link | Site Collection Admin link generated and revoked | **Pass** |

---

## 11. Evidence Index
* `ID-S2-01-standard-user.png`: Baseline standard user profile.
* `ID-S2-02-pim-settings.png`: PIM role settings enforcing 2-hour maximum and justification.
* `ID-S2-03-pim-activation.png`: Active assignment validation showing successful role elevation.
* `ID-S2-04-denied-admin.jpg`: Negative test proving least privilege (User creation denied).
* `ID-S2-05a-site-owners.png`: Project Phoenix mapped to Entra security groups (Owners).
* `ID-S2-05b-site-members.png`: Project Phoenix mapped to Entra security groups (Members).
* `ID-S2-05c-site-visitors.png`: Project Phoenix mapped to Entra security groups (Visitors).
* `ID-S2-06-restricted-library.png`: Document library showing broken inheritance and exclusive Owner access.
* `ID-S2-07-disabled-user.png`: Offboarded user account transitioned to disabled state.
* `ID-S2-08a-drive-limitation.png`: Documentation of uninitialized lab drive state.
* `ID-S2-08b-drive-recovery.png`: Temporary Site Collection Admin link generated for data recovery.
* `ID-S2-08c-drive-cleanup.png`: Verification of temporary access removal.
* `ID-S2-09-admin-ca-policy.png`: Configuration of administrative Conditional Access policy in Report-only state.

---

## 12. Problems Encountered and Troubleshooting
* **Issue:** Attempting to recover data for a targeted offboarded lab user (`Cameron White`) resulted in a system error stating the OneDrive was not set up.
* **Root-Cause Analysis:** In a synthetic lab environment, user accounts that have never actively logged in do not trigger the backend SharePoint/OneDrive provisioning sequence, leaving the drive uninitialized.
* **Resolution:** Documented the platform limitation (`ID-S2-08a-drive-limitation.png`) and pivoted the recovery operation to a "lived-in" test user (`Alex Wilber`) with an initialized data structure, successfully generating and subsequently revoking the temporary access link.

---

## 13. Security Outcome
Eliminated standing SharePoint Administrator access, mapped critical collaborative permissions to manageable security groups, and demonstrated a highly secure, non-persistent method for offboarding data recovery.

---

## 14. Production Recommendations
1. Enforce PIM approval workflows for highly sensitive roles (e.g., requiring secondary administrator approval).
2. Transition `CA-Admin-Strong-Authentication` to enforcement mode following an initial 14-day audit period.
3. Configure Entra ID Access Reviews to audit PIM eligibility assignments quarterly.
