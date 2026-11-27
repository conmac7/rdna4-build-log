# Project Definition Document
## Slot Game — Desktop (Steam) and Meta Quest 3

**1. Document Control**

| Field | Value |
|---|---|
| Document title | Project Definition Document — Slot Game |
| Document ID | PDD-SLOT-001 |
| Version | 1.0 |
| Status | Draft — pending stakeholder review |
| Date | 27 November 2026 |
| Author | [TBD] |
| Document owner | [TBD] |
| Approver | [TBD] |
| Classification | Internal |

Revision history

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | [TBD] | [TBD] | Initial draft from confirmed project inputs |
| 1.0 | 27 November 2026 | [TBD] | Consolidated confirmed scope; submitted for stakeholder review |

Status legend:
- **Confirmed** — established by the project sponsor, or a verifiable external fact.
- **Proposal** — a recommended position that has not been approved.
- **TBD** — undecided; requires a stakeholder decision.

**2. Executive Summary**

This document defines a new slot game project. The following facts are confirmed: the product is a slot game; it will run on desktop PCs distributed through Steam; and it will support the Meta Quest 3 VR headset. All other project parameters — technology stack, slot mechanics, financial parameters, content, timeline, and budget — are undecided and are marked TBD throughout this document.

The purpose of this document is to fix the confirmed scope, state explicitly what is out of scope, and enumerate the decisions required from stakeholders before design and planning can proceed. It is a baseline for stakeholder review, not a game design specification. No proposal in this document is an approved requirement, and no requirement appears here that has not been confirmed, proposed, or marked TBD.

**3. Project Vision**

[Proposal — not approved] Deliver a single engaging slot game experience, available to desktop players through Steam and to VR players on Meta Quest 3. The game should be approachable for desktop players and comfortable to play in VR. The vision is directional; it will be finalized once product positioning, theme, and target audience are decided (Section 13) and will then be submitted for formal approval.

**4. Project Objectives**

1. Deliver the confirmed platforms (confirmed): ship a slot game for desktop PC via Steam and for Meta Quest 3.
2. Resolve open decisions (confirmed): obtain stakeholder decisions on all questions in Section 13 and record them as approved requirements.
3. Establish a defensible plan: convert the approved requirements into a detailed design, schedule, and budget. Target milestones and figures: TBD.
4. Prepare both platforms for store release: meet Steam and Meta certification and review requirements. Concrete criteria: TBD.

**5. Target Platforms**

Confirmed:

| Platform | Distribution / form factor |
|---|---|
| Desktop PC | Steam |
| Meta Quest 3 | Standalone VR headset |

Undecided (TBD):
- Supported desktop operating system(s).
- Minimum and recommended hardware requirements.
- Scope of Quest 3 support: standalone play only, or also PC linking.
- Store, regional, and rating constraints on the Steam and Meta storefronts.

Platform-specific requirements (operating systems, hardware, store policies) will be confirmed as part of the requirements baseline once the open questions in Section 13 are resolved.

**6. Project Scope**

In scope (confirmed):
- Development of a single slot game product.
- Support for the two confirmed platforms.
- Resolution of the open decisions in Section 13 as part of project governance.

Explicitly out of scope at this time:
- Multiplayer features and online services — no such requirement is confirmed; none is assumed.
- Monetization beyond a base game purchase — no such requirement is confirmed; none is assumed.
- Mobile, console, and other VR platforms.

**7. Functional Scope**

The only confirmed functional requirement is that the product is a slot game. All specific mechanics are undecided:

- Reel configuration: TBD
- Payline structure: TBD
- Symbols, themes, and bonus features: TBD
- RTP, volatility, and betting limits: TBD
- In-game currency and economy: TBD
- Session flow (entry, spin, result, exit): TBD
- Platform-specific interaction model (desktop UI versus VR interaction): TBD
- Localization and accessibility: TBD

Input methods (mouse and keyboard for desktop; hand tracking or controller for VR) are TBD pending the platform and interaction model decisions. A detailed functional specification will be produced as a separate document after the relevant stakeholder decisions.

**8. Non-Functional Considerations**

The following categories apply and require defined requirements (currently TBD):
- Performance — frame rate and input latency targets; VR comfort and latency constraints on Quest 3.
- Usability — accessibility of the control scheme for desktop and VR.
- Reliability — stability, error handling, and crash recovery.
- Maintainability and testability — architecture and testing strategy.
- Compliance — the product type (for example, real-money gambling versus entertainment) determines regulatory obligations and age-rating treatment; undecided. Platform store policies for Steam and Meta will additionally apply to the approved product.

**9. Technical Constraints**

- No game engine has been selected (TBD); no assumption is made in this document.
- No programming language has been selected (TBD).
- The confirmed platform set requires support for two significantly different runtime environments (desktop and standalone VR).
- Meta Quest 3 is a standalone device; any Quest 3 build must run on-device.
- Integration with the Steam distribution platform is required by the confirmed distribution channel; Meta platform integration is required by the confirmed platform.
- Development tooling, source control, and CI/CD: TBD.

**10. Dependencies**

1. Stakeholder decisions on the open questions in Section 13 — prerequisite to design work.
2. Engine and technology selection — prerequisite to architecture and staffing.
3. Steam and Meta developer/publisher accounts and store setup — responsible party: TBD.
4. Regulatory determinations, if the product involves real-money play — TBD.
5. Content production (art, audio, copy) — planning: TBD.

**11. Risks**

| # | Risk | Consequence | Mitigation |
|---|---|---|---|
| R1 | Core mechanics and financial parameters remain undecided | Schedule, budget, and scope cannot be baselined | Track open questions with named owners and target dates |
| R2 | Technology stack unselected | Architecture and staffing blocked | Run a documented technology selection before architecture work begins |
| R3 | Divergent desktop and VR experience | Increased development and testing effort | Define shared game rules; allow platform-adaptive presentation |
| R4 | Regulatory obligations unknown until product type is decided | Potential scope, design, and market restrictions | Resolve product type early; seek regulatory advice if real-money play is intended |
| R5 | VR performance and comfort limits on Quest 3 | May constrain game design | Establish VR performance budgets early; validate on target hardware |
| R6 | Store certification and approval timelines (Steam, Meta) | May affect release schedule | Plan certification and store review lead time in the release schedule |

**12. Assumptions**

- A1: A single game product will serve both platforms (shared design), rather than two separate products. To be confirmed.
- A2: Both platforms are subject to their respective store certification and approval processes.
- A3: This document contains only confirmed facts, clearly marked proposals, and items explicitly marked TBD.

**13. Open Questions**

Stakeholder decisions are required on the following; each must be assigned a decision owner and target date before planning can be baselined:

1. Product type: real-money gambling, social/free play, or entertainment-only? (Determines regulatory scope.)
2. Technology: game engine, programming language, and tooling?
3. Gameplay: reel configuration, paylines, symbols, features, RTP, volatility, betting limits?
4. Positioning: theme, art style, target audience, and age rating?
5. Platforms: which desktop operating system(s)? Standalone-only Quest 3, or PC linking as well?
6. Commercial: pricing and monetization model?
7. Program: timeline, budget, team, and delivery model (internal versus outsourced)?
8. Localization: which languages are in scope?

Answers to these questions will be recorded in this document as confirmed requirements at its next revision.

**14. Acceptance Criteria for the Project Definition Document**

This document will be considered approved when:
1. All 14 sections are present and complete.
2. All confirmed facts are consistent with the approved project information, and no unapproved requirement is presented as confirmed.
3. All undecided items are explicitly marked TBD.
4. All open questions in Section 13 are assigned to a decision owner.
5. Terminology is consistent throughout.
6. The document is reviewed and signed off by the named approver in Section 1.
