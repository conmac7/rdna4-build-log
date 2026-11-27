# Project Definition Document

**Project:** Slot Game — Desktop PC and Meta Quest 3 VR
**Document ID:** PDD-SLOT-001
**Version:** 0.1 (Draft)
**Date:** 27 November 2026
**Status:** Draft — pending stakeholder review

## 1. Document Control

**Purpose.** This document establishes the formal definition of the new slot game project. It records the confirmed project information, distinguishes confirmed facts from proposals and unknowns, and identifies the decisions that must be taken before detailed planning can begin.

**Status labels.** The following labels are used consistently throughout this document:
- **Confirmed** — agreed, recorded project information.
- **Proposed** — offered in this document, pending stakeholder approval. A proposal is not an approved requirement.
- **TBD** — undecided; a stakeholder decision is required.

**Version history.**

| Version | Date | Author | Change description |
|---|---|---|---|
| 0.1 | 27 November 2026 | [TBD] | Initial draft based on confirmed project information. |

**Approval.**

| Role | Name | Signature | Date |
|---|---|---|---|
| Product Owner | TBD | — | — |
| Project Manager | TBD | — | — |
| Technical Lead | TBD | — | — |

**Maintenance.** This document must be updated whenever a TBD item is resolved or a confirmed fact changes. Any updated version must be re-reviewed before it is used as a basis for planning.

## 2. Executive Summary

This project develops and releases a slot game. Three platform decisions are confirmed: the game will support desktop PCs; the desktop version will be distributed through Steam; and the game will support Meta Quest 3 VR.

Beyond these decisions, the project is in its definition phase. The slot game's mechanics — including reel configuration, paylines, RTP, volatility, betting limits, and other financial parameters — are not yet defined. The technology stack, including the game engine and programming language, is not yet selected. Multiplayer features, online services, and monetization are not part of the confirmed scope.

The purpose of this document is to establish a shared, accurate baseline of what is known and to surface the decisions stakeholders must make before requirements specification can proceed.

## 3. Project Vision

*Status: Proposed — requires stakeholder approval.*

To deliver a slot game that players can enjoy on desktop PCs and in virtual reality on the Meta Quest 3, providing the same slot gameplay across both environments.

## 4. Project Objectives

Objectives derived from confirmed information:

1. Deliver a playable slot game on desktop PC. (Confirmed)
2. Distribute the desktop version through Steam. (Confirmed)
3. Deliver a playable Meta Quest 3 VR experience of the game. (Confirmed)
4. Resolve the open questions in Section 13 so that detailed functional and non-functional requirements can be specified. (Process objective)

Business objectives (target audience, market position, financial targets): **TBD.**

## 5. Target Platforms

| Target | Status | Detail |
|---|---|---|
| Desktop PC | Confirmed | Supported desktop operating system(s): TBD |
| Steam | Confirmed | Distribution channel for the desktop version |
| Meta Quest 3 (VR) | Confirmed | Other Meta Quest devices: outside confirmed scope |

Support for any desktop operating system other than the one or ones ultimately selected, and for Meta Quest hardware other than the Quest 3, is not confirmed.

## 6. Project Scope

**In scope (confirmed):**
- Development of the slot game
- Support for desktop PCs
- Distribution of the desktop version through Steam
- Support for Meta Quest 3 VR

**Not in confirmed scope.** The following are not assumed by this project and are out of scope unless stakeholders explicitly approve them:
- Multiplayer features
- Online services (account systems, cloud persistence, live content)
- Monetization (release or pricing model, in-game purchases, advertising)
- Platforms or devices other than those listed in Section 5
- Specific slot mechanics and financial parameters (see Sections 7 and 13)

## 7. Functional Scope

The confirmed functional fact is that the deliverable is a slot game. The slot gameplay definition itself is not decided. The following elements are all **TBD** and require a product decision:

- Reel configuration (number of reels and rows)
- Payline configuration
- Symbol set and special symbols
- Bonus or side features
- Return-to-player (RTP) target
- Volatility / risk profile
- Betting limits and denomination structure
- Win and payout structure

These parameters must be finalized before functional requirements can be written. This document deliberately proposes no specific values.

**Desktop (Steam) functionality.** The required set of Steam platform integrations is not yet selected. The baseline feature set, including which Steam features, if any, the game will use, is TBD.

**VR functionality.** The intended VR interaction model for Meta Quest 3 — including how the player initiates spins and how the game is presented in the headset environment — is TBD.

## 8. Non-Functional Considerations

The following areas are identified but not yet defined. All targets are **TBD** pending stakeholder decision:

- **Performance:** frame-rate, latency, and responsiveness targets for desktop and VR.
- **VR comfort:** comfort settings (for example, comfort vignette and motion conventions) for the Quest 3 experience.
- **Localization:** target languages and regions.
- **Accessibility:** player accessibility requirements.
- **Content rating:** target age and content rating. Slot games may be subject to age restrictions and platform content policies; this requires an early decision.
- **Reliability and stability:** acceptable defect and crash-rate levels at release.

## 9. Technical Constraints

**Confirmed constraints:**
- The desktop version must be deliverable through Steam and must satisfy Steam's submission requirements for the desktop operating system(s) it supports.
- The VR version must run on Meta Quest 3 hardware and must satisfy the applicable Meta Quest application submission requirements.

**Undecided items (deliberately not assumed):**
- Game engine: not selected
- Programming language: not selected
- Build, test, and deployment tooling: not selected
- Development hardware and test-device strategy: TBD

The engine and language will be selected deliberately and recorded once decided. This document does not assume either.

## 10. Dependencies

| Dependency | Type | Status |
|---|---|---|
| Stakeholder decisions on the questions in Section 13 | Internal | Required before detailed planning |
| Final slot game definition (mechanics, parameters) | Internal | TBD |
| Steam developer account and Steamworks access | External | Required for Steam distribution |
| Meta developer account and Quest development tooling | External | Required for Quest 3 development and distribution |
| Team and role assignments | Internal | TBD |

## 11. Risks

Preliminary assessment; likelihood and impact will be validated at the project risk review.

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| 1 | Dual-target complexity: maintaining both desktop and standalone VR builds increases development and test effort. | Medium | High | Define a shared game core and platform-specific adaptation early. |
| 2 | Late finalization of the slot definition (RTP, volatility, betting limits) causes downstream rework. | High | High | Require stakeholder decision before the requirements phase. |
| 3 | Regulatory and content-rating issues: slot games may be subject to age restrictions and platform content policies, including Steam's gambling-content rules. | Medium | High | Conduct an early legal and platform-policy review. |
| 4 | Non-compliance with Steam or Meta submission requirements delays release. | Medium | Medium | Review platform submission checklists during definition. |
| 5 | Late technology selection invalidates earlier design or tooling decisions. | Medium | Medium | Select the engine and language before detailed design. |
| 6 | Scope drift: unconfirmed features (online services, monetization) are pulled into scope without a decision. | Medium | Medium | Enforce the scope boundary in Section 6; changes only via documented decision. |

## 12. Assumptions

The following assumptions underpin this document. Each must be validated; if any is invalidated, this document must be revised.

1. The game will be a single-player experience at launch; multiplayer, online services, and monetization remain out of scope unless stakeholders explicitly introduce them.
2. The desktop and VR versions will share the same slot game definition (same mechanics and parameters).
3. The required third-party access (Steam developer program, Meta developer program) can be obtained within the planned timeline.
4. A project team with the necessary skills will be assigned. (Roles: TBD)

## 13. Open Questions

The following questions require stakeholder decisions and will block detailed planning until resolved.

1. **Desktop operating system.** Which desktop operating system(s) must be supported?
2. **Release/pricing model.** What is the intended release model on Steam (for example, free or paid)?
3. **Slot definition.** What are the reel configuration, payline configuration, RTP, volatility, betting limits, and payout structure? Who owns the definition?
4. **Audience and rating.** What is the target audience and target content/age rating?
5. **Steam features.** Which Steam platform features, if any, are required?
6. **VR interaction.** What is the intended VR interaction model on Meta Quest 3?
7. **Technology.** Which game engine and programming language will be used?
8. **Performance targets.** What are the performance targets for desktop and VR?
9. **Localization.** What are the target languages and regions?
10. **Timeline and budget.** What are the planned milestones, schedule, and budget?
11. **Release strategy.** Will both platforms ship simultaneously or in phases?

## 14. Acceptance Criteria for the Project Definition Document

This document is accepted when all of the following are true:

1. All confirmed project information is recorded accurately and without alteration.
2. Every undecided item is explicitly marked as TBD.
3. No proposal is stated as an approved requirement.
4. All open questions requiring stakeholder decisions are listed in Section 13.
5. The Product Owner, Project Manager, and Technical Lead have reviewed the document.
6. The approvers have signed the approval table in Section 1.

Upon acceptance, this document becomes the baseline reference for project planning. Subsequent changes follow the review and update process defined in Section 1.
