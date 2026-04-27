0. Round-2 Executive Diagnosis
   ==============================

Round 2 targets the **mechanical closure** of the guarantee production system. While Round 1 established the vocabulary of "obligations" and "evidence," the system remains a "soft" construct because it lacks the formal machinery to prevent subjective human judgment from infiltrating the release gate. This round must replace normative descriptions with a **computable assurance logic**—specifically by deriving thresholds from utility-loss functions, defining how local evidence joins into system-level claims, and hardening the release gate with automated admissibility rules.

1. P0/P1 Repair Map
   ===================
* **P0: Subjective Threshold Collapse.** Without formal derivation, "Acceptance Contracts" are merely versioned opinions. If thresholds are not mathematically tied to utility-loss or risk-caps, the evidence chain fails as a defensible proof.

* **P0: Compositional Vacuum.** The current bluebook assumes system safety is an aggregate of module safety. Without a **compositional grammar**, the "Unified Control Plane" cannot prove that a safe tool-call was not triggered by a malicious retrieval slice.

* **P1: Semantic Compilation Gap.** Natural-language obligations (e.g., "protect user privacy") are disconnected from runtime triggers (T1-T3). This creates a "gray zone" where claim discipline becomes rhetorical rather than executable.

* **Impact Map:** Failure to fix these load-bearing points will cause the **Release Evidence Pack** to fail under adversarial audit, as claims will lack a traceable lineage back to foundational theory.
2. Threshold Derivation Reconstruction
   ======================================

**Formalization Direction:** Move from "expert-set thresholds" to **Constrained Risk Minimization (CRM)**.

**Minimal Theory Candidate:** A threshold $\theta$ is defensible if it is the result of an optimization problem:

$$\min L(U) \text{ subject to } P(\text{Harm} | \theta) \leq \delta$$

Where $L(U)$ is the utility loss and $\delta$ is the risk cap defined in the **Acceptance Contract**.

**Rewrite Instructions (Chapter 5.6):**

* **Delete:** Language suggesting thresholds are "expert-driven" or "agreed upon" by committees.

* **Add:** A "Threshold Selection Logic" section. Distinguish between **Evidence-Constrained Thresholds** (derived from historical p99 failure rates) and **Optimization-Derived Thresholds** (derived from the joint target of security, utility, and latency).

* **Theorem Candidate:** _The Threshold Defensibility Theorem_—A threshold is contractible if and only if it minimizes utility loss while satisfying the safety invariants defined in the jurisdictional obligation baseline.
3. Compositional Security Reconstruction
   ========================================

**Compositional Grammar:** Define the system guarantee $G_{sys}$ as a join $(\oplus)$ of component evidence $E_i$ over a shared `correlation_id`.

$$G_{sys} \equiv E_{in} \oplus E_{ret} \oplus E_{gen} \oplus E_{tool}$$

**Legal vs. Illegal Evidence Compositions:**

* **Legal:** All components cite the same `policy_version` and `correlation_id`; rationale codes are mutually consistent across the chain.

* **Illegal (The "Shadow Bypass"):** $E_{gen}$ (generation) is marked safe, but $E_{tool}$ (tool use) evidence is null for a request that invoked high-impact side effects.

* **System-Level Rule:** A system-level claim is invalidated if any node in the evidence graph for a single `correlation_id` is missing or fails the `policy_version` check.
4. Obligation Compilation Reconstruction
   ========================================

**Obligation-to-Trigger Chain:**

1. **Natural-Language Obligation:** "Prevent unauthorized data exfiltration".

2. **Obligation Class:** Confidentiality / Data Privacy.

3. **Policy Field:** `allowed_retrieval_domains`, `redaction_sensitivity_level`.

4. **Event Type / Trigger:** `T1 (Policy Conflict)` if a tool attempts to send high-sensitivity data to an unlisted domain.

5. **Runtime Decision:** `D4 (Hard-Stop)` or `D3 (Redact)`.

6. **Rationale Code:** `ERR_PRIV_EXFIL_BLOCKED`.

7. **Evidence Item:** Joined record in the **Evidence Plane**.

8. **Claim Template:** "Under version $V$, exfiltration was prevented for $N$ cases with $100\%$ replay fidelity".

**Compilable vs. Non-Compilable:**

* **Compilable:** Deterministic constraints (regex, allowlists, metadata tags).

* **Non-Compilable:** Open-ended "harmlessness" or "helpfulness." These must be downgraded to **Statistical Claims** with explicit error bounds in the claim discipline.
5. Admissibility Logic Reconstruction
   =====================================

**Admissibility Rules for Release Gating:**

* **Rule 1 (Provenance):** Any evidence entry $E$ where $Source(E) \in \{\text{Track B}, \text{Ad-hoc Test}\}$ is strictly inadmissible for **Track A** release conclusions.

* **Rule 2 (Closure):** Evidence is admissible only if it includes the complete join of minimal three-plane fields.

**Release Gate Invariant:**

$$\forall E \in \text{Release Evidence Pack}, \text{Metadata}(E).status == \text{"Deployable"} \wedge \text{Metadata}(E).policy\_version == \text{Current\_Release\_Version}$$

**Anti-Abuse Mechanism:** The release gate must be a **hard-coded validator** within the DTL Evidence Engine that cryptographically rejects any Release Evidence Pack containing unversioned artifacts or mismatched `correlation_id` samples.

6. Chapter Rewrite Agenda
   =========================
* **Chapter 1:** Rewrite Section 1.3. Replace "necessity is visible via fractures" with a formal **Necessity Proof for Obligation Closure**.

* **Chapter 3:** Formalize Section 3.4. Define the `Join` operator for the three planes as a **primary system invariant**.

* **Chapter 4:** Rewrite Section 4.7. Define the T1-T3 triggers as **finite state machine (FSM)** transitions to ensure no "undefined" system states.

* **Chapter 5.6 / 5.8 / 5.10 / 5.11:** Compress the "scoring" narrative. Formalize the **Evidence Pack** as a typed object that must satisfy admissibility rules before governance convergence.

* **Chapter 6.1 / 6.2:** Upgrade "Claim Discipline" from a grammar of prose to a **Claim Type System**. Statements must be "type-checked" against the available evidence regime (White-box vs. Black-box).

* **Chapter 7:** Add a section on "Jurisdictional Admissibility." Define how local regulatory obligations map to **Track A** language gates.

* **Chapter 8:** Formalize the **One DTL Security Program** as a "Compiler for Assurance," where theory is the source code and the bounded contract is the binary artifact.
7. Mandatory New Insertions
   ===========================
* **One Theorem:** _The Compositional Evidence Soundness Theorem_—A system claim is sound if and only if all sub-component evidence joins satisfy temporal and semantic consistency within the request budget $S1$.

* **One Figure:** **The Evidence-Join Dependency Graph**. Shows how `correlation_id` threads through the Gateway, Retrieval, Kernel, and Tool layers to produce the **Release Evidence Pack**.

* **One Canonical Case:** "Cross-Tenant Tool Hijacking via Retrieval." Demonstrates how a missing evidence join in the retrieval path allows an adversary to escalate privileges, and how the **Release Gate** would catch this as an "Incomplete Evidence" error.

* **One Anti-Abuse Test Suite:** **"The Marketing Over-Claim Test."** A suite that takes an ambitious claim from a release note and attempts to compile it back to **Track A** evidence; if it points to **Track B** (mechanism-only) results, the gate fails the release.

If Only Three Fixes Survive
===========================

1. **Admissibility Logic for Release Gating (Chapter 5/6):** Highest ROI. It transforms the system from a "suggestion box" into a **governance machine** that executives cannot bypass.

2. **Threshold Derivation via Utility-Loss (Chapter 5.6):** Crucial for scientific defensibility. It stops "safety" from being an arbitrary bar and makes it a **business-economic decision**.

3. **The Compositional Evidence Grammar (Chapter 3/4):** Essential for reviewer resistance. It solves the "Composite System" problem, proving the **Control Plane** can handle multi-step workflows.

A single relevant follow-up question to guide the conversation forward:

_How should the DTL institutional authority be formally mapped into the CI/CD pipeline to ensure that the "Release Gate" is a technical blocker rather than a manual approval step?_
