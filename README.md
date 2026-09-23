# Security Control Gap Assessments

Three governance, risk, and compliance assessments across different industries and regulatory
regimes. Each takes an organization's assessed security posture, identifies where it fails to
meet a controlling framework, rates the risk, and specifies mitigations tied to the control that
was missed.

Detection and response work gets the attention, but somebody has to answer why a control exists,
what happens when it is absent, and which of twelve gaps gets funded first. These three cover
that skill across federal (NIST SP 800-53 / RMF), payment and privacy (PCI DSS / GDPR), and cloud
migration (FISMA) contexts.

## Tools and frameworks used

| Framework | Applied in |
|---|---|
| NIST SP 800-53 Rev. 5 | Control selection and gap analysis, all three assessments |
| NIST Risk Management Framework | Assessment structure and risk rating |
| NIST SP 800-30 Rev. 1 | Risk assessment methodology |
| PCI DSS v4.0 | Healthcare POS and retail payment environments |
| GDPR | Retail e-commerce collecting EU personal data |
| FISMA | Federal contractor cloud migration |
| CVSS v3.1 | Vulnerability severity input |
| Microsoft Azure | Target platform for the cloud security plan |

---

## 1. Healthcare — NIST 800-53 control gap analysis

**Organization:** Fielder Medical Center, a healthcare provider handling PII and processing card
payments, subject to federal cybersecurity requirements.

### Gaps identified

**Identity and access management.** No secure authentication mechanism for physicians or for the
government agencies the center exchanges data with. Without authentication, least privilege
cannot be enforced — every downstream access control rests on an identity the system cannot
verify.

**Endpoint protection.** Workstations were running unlicensed antivirus or none at all. The
point-of-sale system had no antivirus solution, which independently fails PCI DSS requirements
for protecting cardholder data.

**System hardening.** No multifactor authentication anywhere in the environment, on a system
whose primary function is storing sensitive PII.

### Control ratings

Five NIST SP 800-53 controls, each rated on impact and likelihood in the organization's context:

| Control | Rating | Reasoning |
|---|---|---|
| **AC-6** Least Privilege | **High** | Not enforced across systems. Users hold permissions exceeding their role, giving access to sensitive records they are not authorized to view. Compounds every other access gap. |
| **CA-5** Plan of Action and Milestones | **Moderate** | No formal process for documenting remediation. Leaves known weaknesses unpatched and unprioritized, and fails federal compliance requirements outright. |
| **CA-7** Continuous Monitoring | **High** | No monitoring capability. Threat activity would go undetected indefinitely — the absence that turns a contained incident into a breach. |
| **RA-3** Risk Assessment | **High** | No updated assessment for the new system. Leadership cannot prioritize investment without knowing what the exposure is. |
| **RA-7** Risk Response | **Moderate** | No documented procedure for responding to identified risks. Identification without a response path produces findings nobody acts on. |

### Why the ratings differ

AC-6, CA-7, and RA-3 rate High because each is **directly exploitable or directly blinding** —
excess privilege is the mechanism of unauthorized access, absent monitoring means no detection,
and an outdated risk assessment means resources go to the wrong place.

CA-5 and RA-7 rate Moderate because they are **process gaps that amplify other failures** rather
than causing compromise on their own. A POA&M does not stop an attacker; its absence means the
weaknesses that do stop being tracked. Distinguishing the control that gets exploited from the
control that lets exploitation go unmanaged is what keeps a remediation budget pointed at the
right things.

## 2. Retail — PCI DSS and GDPR compliance gaps

**Organization:** SAGE Books, a national bookseller operating retail stores, distribution centers,
and an e-commerce platform serving EU customers.

### Gaps identified

1. **No PCI DSS compliance procedures** despite processing card transactions both in-store and
   online — exposure to financial penalties and cardholder data compromise.
2. **No policies for mobile device management, password protection, or PII handling** — endpoint
   devices ungoverned by any acceptable-use standard.
3. **No dedicated GRC personnel structure.** Nobody owns governance, compliance, or risk
   management, so no framework stays current regardless of what is written down.
4. **No GDPR privacy protections** despite collecting and processing EU customer data through the
   e-commerce platform.
5. **Deficient incident response plan, security awareness training, and business continuity
   plan** — no formalized training schedule, expanding the human attack surface.

### Mitigations

| Gap | Control | Standard |
|---|---|---|
| Cardholder data exposure | TLS for data in transit, AES for data at rest | PCI DSS Req. 3, 4 |
| Flat network | Segment payment systems from the corporate network with firewalls | PCI DSS Req. 1 |
| Unknown exposure | Scheduled vulnerability assessments and penetration testing | PCI DSS Req. 11 |
| No detection capability | SIEM for centralized log collection and correlation | PCI DSS Req. 10 |
| Excess access | Least privilege via IAM and role-based access control, reviewed on role change and departure | PCI DSS Req. 7 · GDPR Art. 32 |
| EU data subject rights | Privacy protections per GDPR Ch. 3 | GDPR Art. 12–23 |

The controls were chosen to satisfy both regimes where they overlap. Access control and
encryption serve PCI DSS Requirements 3, 4, and 7 and GDPR Article 32 simultaneously — in a
multi-regime environment, mapping one control to several obligations is what keeps a compliance
program affordable.

## 3. Federal contractor — cloud security implementation plan

**Organization:** SWBTL LLC, a nationwide logistics and document delivery company, ~2,000
employees, founded 1977. Operates four leased US data centers running internally developed and
vendor-written software.

### Business drivers

Data center costs were rising, with service interruptions and security risk alongside them. The
company holds **US government contracts** and processes **daily card transactions**, putting it
under **FISMA** and **PCI DSS** simultaneously, with a **NIST SP 800-53 assessment** approaching
during an in-progress **Microsoft Azure migration**.

### Why this case is different

The other two assess a static environment. This one plans security **for an environment that does
not exist yet**, under an audit deadline, with regulatory obligations that follow the workloads
into the cloud.

Three constraints shaped the plan:

- **Migration is not a compliance reset.** FISMA and PCI DSS obligations move with the data.
  Controls have to be in place at cutover, not retrofitted after.
- **The shared responsibility model reallocates controls; it does not remove them.** Azure covers
  physical and hypervisor security. Identity, data protection, configuration, and monitoring
  remain the customer's, and assuming otherwise is the most common cloud compliance failure.
- **An 800-53 assessment during migration means controls need evidence, not just design.**
  Logging, configuration baselines, and access reviews have to produce artifacts an assessor can
  test.

---

## Results

| Assessment | Sector | Frameworks | Focus |
|---|---|---|---|
| Fielder Medical Center | Healthcare | NIST 800-53, RMF, PCI DSS | Control-level gap analysis with risk ratings |
| SAGE Books | Retail / e-commerce | PCI DSS, GDPR, NIST | Multi-regime compliance and policy development |
| SWBTL LLC | Logistics / federal contractor | FISMA, PCI DSS, NIST 800-53 | Cloud migration security architecture |

Together they cover control selection, risk rating and justification, multi-regime compliance
mapping, and security planning for an environment under active change.

## Lessons learned

**Rate risk in context, not by control family.** AC-6 and CA-5 are both access-and-assessment
controls; one rated High and the other Moderate because of what each absence does in that
specific environment. A rating that ignores the organization is a checklist, not an assessment.

**Separate controls that get exploited from controls that let exploitation persist.** Missing MFA
is how an attacker gets in. A missing POA&M is why the weakness was still there. Both need
fixing; only one of them buys time.

**Map one control to every obligation it satisfies.** Encryption and least privilege serve PCI
DSS and GDPR at once. Programs that treat each regime as a separate project buy the same control
twice and maintain it inconsistently.

**Compliance obligations migrate with the workload.** The FISMA and PCI DSS scope that applied in
SWBTL's data centers applies in Azure. The shared responsibility model changes who implements
which control; it never reduces the set that must exist.

**Write the verification, not just the recommendation.** "Implement least privilege" is not
actionable. "Implement RBAC, review on role change and departure, evidence the review quarterly"
is something an assessor can test and an owner can be held to.

---

*Completed as coursework for the M.S. Cybersecurity and Information Assurance program at Western
Governors University. All organizations and scenarios are fictional case studies supplied by the
university; the analysis, control ratings, and recommendations are my own. The original graded
submissions and the university-supplied case documents are intentionally not published here.*
