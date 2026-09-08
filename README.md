# HSRW UniCard

A hybrid student credential that merges student identification, Mensa payment, library access and VRR/NIAG transit authentication into one ID, issued both as a physical NFC smart card and as a digital wallet pass.

The project answers three questions in order. Can four fragmented campus services be consolidated without replacing anyone's back end? What does that consolidation have to look like for students who live on their phone and for students who cannot rely on one? And what actually stops such a system from reaching the campus? The first two were answered with a working prototype and an integration architecture. The third turned out to be the real finding.

Project Management and Intercultural Competence · Summer Semester 2026 · Rhine-Waal University of Applied Sciences  
B.Sc. Mobility and Logistics · Group 6

## Results

| Goal | Result | Assessment |
|---|---|---|
| Integrate four services in one credential | All four designed and demonstrated, API core specified | Achieved at design level |
| Replace three or more cards with one | One credential demonstrated, no campus deployment | Partially achieved |
| Integrate with existing live systems | Endpoints specified against simulated data | Not achieved |
| Deliver architecture this semester | Architecture delivered, beta depends on approval | Achieved, beta at risk |

| Measure | Planned | Actual |
|---|---|---|
| Effort | 440 h | 474 h (+7.7 %) |
| Evaluated cost at €100/h | €44,000 | €47,400 |
| Duration | 13 Apr – 2 Aug 2026 | on schedule internally |
| Critical path | — | 27 time units, σ = 1.56 |

Cash spend was zero. No hardware was bought and hosting was free; the €47,400 is a valuation of student hours for the cost-control exercise, not an expenditure.

The highest-scored risk in the register, bureaucratic inertia and approval cycles at probability 4 × impact 4 = 16, is the one that materialised. That is the single most useful result the project produced.

## The problem

Students carry separate cards or apps for the library, for Mensa payments and for public transport, plus a paper enrolment certificate that carries no photograph. Because the certificate cannot prove identity on its own, a passport or national ID is often required alongside it during examinations or ticket checks.

A phone-only solution does not close this. A dead battery or a broken device would block access to essential services, which is why the design keeps a physical credential rather than treating it as a legacy fallback. The mantra was digital-first by design, physically robust by necessity.

The original idea also covered credit, lockers, parking and event ticketing. Scope was cut to the four essential services that could be delivered within one semester. In hindsight the cut was correct but was never recorded in a formal out-of-scope list, which is a documentation failure rather than a design one.

## The design

| Service | Delivered through |
|---|---|
| Student identification with digital photo | NFC card and wallet pass |
| Mensa payment and balance | API to Studierendenwerk systems |
| Library access | API to the library system |
| VRR/NIAG transit authentication | Wallet pass with physical fallback |

The architecture is a decoupled API gateway. The UniCard speaks to existing databases through an abstraction layer, so no partner has to rebuild a back end to participate. That decision came directly out of the market analysis: the services already exist and work, what is missing is a single front door to them.

GDPR/DSGVO handling is built into the gateway rather than bolted on: tokenisation at the API layer, minimised local storage, service data kept compartmentalised, verifiable permissions and full logging.

The prototype covers wallet provisioning, identity and library access, Mensa balance and payment, transit validation and the physical backup path. It runs on simulated demonstration data, because the institutional approvals needed to reach the HSRW, Studierendenwerk and transit databases were never obtained.

## Why hybrid

Two personas drove the form factor, and they disagree with each other.

| Persona | Pain point | Requirement |
|---|---|---|
| Frequent traveller | Carries several service cards, cannot depend on a charged phone | Physical NFC card, fast Mensa transaction, VRR ticket included |
| Smartphone-first international student | Finds the current system fragmented, dislikes paper certificates, has no digital backup | Wallet integration, digital photo ID, real-time information |

Neither persona is served by choosing one form factor. The hybrid card-and-wallet credential exists because of that conflict, not because two channels are nice to have. The personas came from observation and informal conversation with peers, so they are design instruments and not a statistical sample.

## Analysis

**SWOT with weighted IFAS/EFAS scoring.** IFAS came out at 2.60 against a 2.50 midpoint, EFAS at 2.40. Internally solid, externally exposed. The strengths are first-hand user insight, a clear hybrid value proposition and disciplined scheduling; the weaknesses are API and legacy-system complexity, limited time and resources, and no mandate over the institutions that own the data. Externally, digital transformation in higher education and a scalable modular architecture sit against the GDPR compliance burden, administrative inertia and vendor lock-in.

The conclusion that follows from those two numbers is the thesis of the whole project: what stands between this concept and a campus rollout is institutional approval, not interface design.

**Stakeholder analysis.** Six groups, mapped on power and interest. Students have high interest and low formal power. University administration, university IT, the Studierendenwerk and the transit partners hold the power, because implementation needs their approval and their interfaces. So students belong in a usability pilot, and the powerful groups need managing closely with a named sponsor secured early.

**Ishikawa root-cause analysis.** The visible symptom is the number of cards. The 6M breakdown traces it to separate registration procedures, incompatible databases, ageing readers, decentralised campus operations and the absence of real-time balance visibility. The Measurement branch was the productive one: it showed that balance and transaction history were nowhere available as a single view, and that gap became a prototype feature.

**Risk register.** Rated probability × impact on a 1–5 scale, with 12 the threshold for high priority.

| Risk | P | I | Score | Mitigation |
|---|---|---|---|---|
| Bureaucratic inertia and approval cycles | 4 | 4 | 16 | Secure a sponsor early, run approvals in parallel |
| Accessibility and device dependency | 3 | 4 | 12 | Keep the NFC card alongside the wallet, test varied users |
| GDPR/DSGVO exposure | 2 | 5 | 10 | Tokenisation, reduced storage, DPO assessment |
| Conflicting stakeholder priorities | 3 | 3 | 9 | Decoupled API with autonomous services |

## Planning

The schedule was built as a six-phase Gantt chart over 474 hours, with work packages assigned per member and a per-member cost sheet.

| Phase | Dates | Hours |
|---|---|---|
| Initiation and ideation | 13–16 Apr 2026 | 32 |
| Kick-off presentation | 16–19 Apr 2026 | 54 |
| Interim presentation | 21 Apr – 10 May 2026 | 84 |
| Core research and analysis | 12–27 May 2026 | 44 |
| Prototyping and architecture | 28 May – 14 Jun 2026 | 68 |
| Finalisation and documentation | 15 Jun – 2 Aug 2026 | 192 |

Fourteen activities were estimated three-point and scheduled with PERT, tₑ = (o + 4m + p) / 6 and σ² = ((p − o) / 6)². The critical path runs B–D–H–K–N: problem definition, market analysis, Gantt and timeline planning, prototype development, and final documentation and controlling. Expected length 27 units with variance 2.44, giving roughly 50 % confidence at 27 units, 90 % at 29 and 97 % at 30. Prototype development carried the largest variance and therefore needed the tightest schedule control.

Planned against actual, the interesting number is not the 34-hour overrun but its distribution. Prototyping underran by 127 hours because live integration never happened, while final documentation overran by 112 hours. The totals almost cancel and hide two large estimation errors in opposite directions.

## Usability testing

Five participants worked through five predefined tasks each: student identification, Mensa, library access, transport ticket and the physical fallback. Participants worked unaided; the observer logged comments, completion time and any assistance given.

| | Independent | Assisted | Not completed |
|---|---|---|---|
| Across 25 task attempts | 20 | 3 | 2 |

All five found the digital student ID without help. Two needed help locating the transport ticket, one needed the physical fallback explained, one could not complete the Mensa task and one could not complete library access. The pattern points at labelling rather than architecture: Transport, Wallet and the service navigation need clearer wording.

One feedback questionnaire came back, averaging 4.6 out of 5. Five task records and a single questionnaire do not support a general statement about acceptance, and the report says so rather than rounding the finding up.

## Lessons learned

Secure a senior institutional sponsor before the project depends on institutional data. The team identified administrative inertia as its top risk and then did not act on it, which is the gap between risk analysis and risk management.

Estimate effort at work-package level, and count report writing and quality assurance as work. The 127-hour underrun on prototyping and the 112-hour overrun on documentation are the same estimation error seen from both sides.

Hold prototype claims to quantitative evidence. Twenty of twenty-five tasks completed unaided is a promising signal, not validation, and the small sample is a limitation to state rather than to bury.

## Outlook

The remaining barriers are institutional rather than intellectual: an institutional sponsor, data access agreements, cooperation with the transit operators and sign-off from the university's data protection officer. Before any wider usability sample, the next iteration needs live API integration, clearer wallet and transit labelling, an improved participant questionnaire and accessibility testing.

## Method and references

The project follows a standard project-management sequence: goal definition, project analysis, scheduling and network planning, execution, controlling against plan, and evaluation.

- Doran, G. T. (1981). There's a S.M.A.R.T. way to write management's goals and objectives. *Management Review*, 70(11), 35–36.
- Ishikawa, K. (1985). *What Is Total Quality Control? The Japanese Way*. Prentice-Hall.
- Malcolm, D. G., Roseboom, J. H., Clark, C. E., & Fazar, W. (1959). Application of a technique for research and development program evaluation. *Operations Research*, 7(5), 646–669.
- Wheelen, T. L., & Hunger, J. D. (2012). *Strategic Management and Business Policy* (13th ed.). Pearson.

The written project report, the kick-off and interim presentations and the analysis workbooks are not published here, because they carry the group members' personal university details. The results, tables and reasoning they contain are reproduced in this README. The documents themselves are available on request.

## Authors

Group 6 — Rhine-Waal University of Applied Sciences, B.Sc. Mobility and Logistics.

| Member | Role | Contribution |
|---|---|---|
| Mohammad Mushfiqur Rahman Joy | Project Manager | Schedule baseline, sprint coordination and stakeholder communication. Initial solution approach, Ishikawa diagram, persona building, the interactive prototype, and the cost and time evaluation. |
| B M Muntasir Fahim | IT Lead | Technical architecture and prototype development. USP and target-group definition, SWOT analysis, the activity network and critical-path analysis, UI/UX wireframing and the technical documentation. |
| Tarek Hasan | Researcher | Market, stakeholder, persona, risk and GDPR research. Business Model Canvas, the visual Gantt chart, the external usability-test protocol and observation sheet, and report-wide quality assurance and cross-review. |
| Gazi Alvi Junaid | Coordinator | Meeting facilitation, documentation control and presentation assembly. Problem definition and goals, sprint timeline and resource planning, the risk register, the GDPR feasibility check, and the final evaluation and next steps. |

Scope, the SMART goal set and the acceptance criteria were agreed jointly by the group.

Academic coursework, published for reference and portfolio purposes. Please cite the authors if you build on the analysis.
