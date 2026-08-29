# User-journey design and validation

Use this reference when a change adds or alters behavior that a human or external consumer must operate. The goal is not to document every click; it is to prove that the capability can be found, completed, and used as part of a real workflow.

## Define the journey contract

Describe the smallest complete journey before choosing implementation details:

1. **Actor and goal** — who acts, what outcome they need, and why it matters.
2. **Realistic start** — the normal landing page, application shell, CLI help, API documentation, integration event, or other place the actor actually begins.
3. **Entry** — the visible navigation, affordance, command, endpoint, or link that exposes the capability to the intended role.
4. **Preconditions** — authentication, permissions, data, configuration, and device state; identify how the actor can satisfy each one.
5. **Critical path** — the meaningful user actions and system responses, including what tells the actor what to do next.
6. **Closure** — the visible completion signal, persisted result, and promised downstream action the result enables.
7. **Recovery** — how the actor corrects invalid input, retries a failure, cancels, returns, or resumes safely.

Use Given/When/Then for observable behavior, but keep the journey steps concrete enough to expose navigation and handoff gaps. Separate behavior from brittle implementation details.

## Design from outside in

- Trace each journey step to a UI surface, command, API contract, permission check, state transition, and feedback mechanism as applicable.
- Verify there is no gap between creating a backend capability and exposing it through the intended entry.
- Treat “the route works when opened directly” and “the endpoint returns 200” as component evidence, not proof of a complete user journey.
- Make the next action apparent through visible state, navigation, documentation, or a stable machine-readable contract.
- Check handoffs across frontend/backend, authentication, services, persistence, asynchronous work, notifications, and external integrations.
- For a modified middle step, inspect at least the immediate upstream entry and downstream consequence; expand farther for risky or cross-cutting changes.
- If the capability is intentionally internal-only, name its real consumer and invocation path instead of inventing a human UI journey.

## Validate like the intended actor

1. Start the product through its supported startup path and establish controlled test data.
2. Begin at the realistic start, not the implementation's private route or final state.
3. Find the entry using what the actor can see or is documented to know.
4. Perform the critical path with user-facing roles, labels, commands, and contracts.
5. At each transition, observe feedback and confirm that the next action is available and understandable.
6. Confirm closure through the visible result and at least one promised downstream use, re-entry, or persistence check.
7. Exercise the most consequential failure or correction path when risk warrants it.
8. Record any setup shortcut. A shortcut may establish an unrelated precondition, but it cannot replace the behavior being accepted.

For browser flows, inspect relevant console and network failures without using internal calls as the sole success oracle. For CLI or API flows, begin at the documented command or public contract and verify the result through the consumer-visible output and subsequent supported operation.

## Journey-completeness review

Before accepting the change, ask:

- Can the intended actor discover or invoke it without developer knowledge?
- Can the actor satisfy every precondition through a supported path?
- Does each step communicate what happened and what comes next?
- Does success produce a durable or immediately usable result?
- Can the actor recover from the likely failure modes?
- Did verification cover the actual entry and closure rather than only the changed component?

Any “no” is a design gap, missing scope decision, or explicit product limitation—not a passing journey.
