# Agent Design Review Checklist

This checklist is intended for formal design reviews of agent-based systems. Its purpose is to evaluate whether a proposed agent is **theoretically well-founded, appropriately scoped, empirically evaluable, and operationally viable in production environments**. A design should satisfy **the majority of criteria prior to implementation** and **all critical criteria prior to external deployment**.

---

## 1. Problem Framing and Scope (Critical)

- ☐ Is the **objective function** of the agent explicitly articulated and empirically testable?
- ☐ Are **success, partial success, and failure conditions** formally specified?
- ☐ Is the use of an **agentic system justified**, as opposed to a single inference call, deterministic pipeline, or conventional software abstraction?
- ☐ Does the task exhibit sufficient **open-endedness, partial observability, or multi-step dependency** to warrant agentic control?
- ☐ Are user expectations calibrated to the **stochastic and fallible** nature of model-driven systems?

**Red flags**: underspecified goals (e.g., “assist the user”), agentic solutions applied to problems better solved with deterministic logic.

---

## 2. Capability Level and Architectural Alignment (Critical)

- ☐ What **agent capability level (0–4)** is strictly required to solve the problem?
- ☐ Is the selected level the **minimum necessary complexity** rather than a maximal or exploratory choice?
- ☐ Is the system architecture explicitly decomposed into:
  - ☐ Model (reasoning and inference)
  - ☐ Tools (external actions and side effects)
  - ☐ Orchestration (control flow and decision logic)
  - ☐ Runtime and Operations (state management, evaluation, deployment)
- ☐ Are boundaries and responsibilities between these components clearly defined and enforceable?

**Red flags**: business or control logic embedded implicitly in prompts; ambiguous ownership of orchestration behavior.

---

## 3. Orchestration and Control Dynamics (Critical)

- ☐ Is the **sense → decide → act → observe → update** control loop explicitly specified?
- ☐ Are termination conditions formally defined (goal satisfaction, failure states, iteration limits)?
- ☐ Are retry semantics, fallback behaviors, and error recovery paths designed in advance?
- ☐ Is deterministic control employed where feasible (routing rules, guard conditions, validators)?

**Red flags**: unbounded execution, uncontrolled retries, or full delegation of control policy to the model.

---

## 4. Tooling and Action Space (Critical)

- ☐ Are the available tools **necessary, sufficient, and minimal** with respect to the agent’s objective?
- ☐ Are tool interfaces (inputs, outputs, side effects) formally specified and validated?
- ☐ Are permissions scoped according to the **principle of least privilege**?
- ☐ Are hallucinated, malformed, or adversarial tool invocations handled safely?

**Red flags**: overly permissive tools, ambiguous tool contracts, or implicit trust in model-generated actions.

---

## 5. Context Engineering and State Management

- ☐ What information is injected into the **per-step context window**, and why?
- ☐ What state is maintained at the **session level** versus persisted as **long-term memory**?
- ☐ Are strategies defined for summarization, pruning, or compression of historical context?
- ☐ Are context growth, cost, and latency explicitly bounded and monitored?

**Red flags**: naïve inclusion of full interaction histories; absence of a defined memory lifecycle.

---

## 6. Multi-Agent System Design (If Applicable)

- ☐ Is the introduction of multiple agents theoretically or empirically justified relative to a single-agent design?
- ☐ Are agents decomposed by **cognitive or functional role** (e.g., planning, execution, evaluation)?
- ☐ Is inter-agent communication structured via explicit schemas or protocols?
- ☐ Are interaction histories shared or isolated by deliberate design choice?
- ☐ Is cross-agent interoperability (e.g., MCP, A2A-style delegation) considered?

**Red flags**: proliferation of agents without role clarity, excessive communication overhead, unclear coordination semantics.

---

## 7. Evaluation Methodology (Critical)

- ☐ Are representative **golden tasks** defined that reflect real-world usage distributions?
- ☐ Are both **final outcomes and intermediate trajectories** evaluated?
- ☐ Are cost, latency, and tool utilization systematically measured?
- ☐ Is structured human feedback incorporated where automated metrics are insufficient?

**Red flags**: reliance on anecdotal testing or evaluation limited to final textual outputs.

---

## 8. Safety, Security, and Compliance (Critical)

- ☐ Are unsafe or disallowed actions explicitly constrained at the orchestration or tool layer?
- ☐ Are all tool invocations auditable, logged, and attributable?
- ☐ Has sensitive data handling been reviewed for compliance and leakage risk?
- ☐ Are prompt injection, tool injection, and cross-context contamination risks addressed?

**Red flags**: implicit reliance on model judgment for safety or lack of forensic traceability.

---

## 9. Production Readiness and Operations

- ☐ Is deployment gated by explicit evaluation thresholds?
- ☐ Is step-level tracing and observability implemented?
- ☐ Are rollback mechanisms and kill switches available and tested?
- ☐ Is there an incident response plan for agent misbehavior or degradation?

**Red flags**: experimental agents exposed to users without operational safeguards.

---

## 10. Cost, Latency, and Scaling Considerations

- ☐ Are explicit cost and latency budgets defined and enforced?
- ☐ Is context size bounded and continuously monitored?
- ☐ Are expensive tools or model calls rate-limited or amortized?
- ☐ Does system performance scale predictably with user volume?

**Red flags**: unbounded loops, unconstrained context growth, or opaque cost drivers.

---

## 11. Maintainability and Evolution

- ☐ Are prompts, tools, and orchestration logic versioned and reviewable?
- ☐ Can major components be tested and evolved independently?
- ☐ Is there a defined process for iterative improvement based on observed behavior?
- ☐ Are learned insights propagated back into system design or memory structures?

**Red flags**: brittle prompt-centric implementations with no ownership or evolution strategy.

---

## Final Review Gate

Prior to approval, reviewers should be able to answer affirmatively:

> “Do we have a principled understanding of how this agent reasons, acts, fails, and recovers—and do we possess the instrumentation to verify that understanding empirically?”

If not, the design should be considered unsuitable for production deployment.

