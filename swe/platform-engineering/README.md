# Platform Ownership And Maintenance

Source note:

- [I was laid off by Atlassian](https://www.youtube.com/watch?v=55pTFVoclvE)
  by Vasilios Syrakis

Related system-design note:

- [Platform engineering: self-service edge load balancing](../../system-design/platform-engineering/README.md)

## Core Lesson

Building a platform is not just building the first working version. The harder
part is keeping it understandable, operable, migratable, and changeable while
people, services, requirements, and compliance expectations keep changing.

## Maintenance Signals

Watch for these signs that a platform area needs attention:

- the same files or templates keep changing for unrelated features;
- every new tenant needs a special case;
- onboarding requires live explanation instead of durable docs;
- only one or two people know how to debug production issues;
- valid configuration can still break traffic in surprising ways;
- teams bypass the platform because the self-service path is unclear or slow;
- migration work is pushed to every dependent team instead of handled with
  reusable tooling.

High churn is not automatically bad, but repeated churn in the same area is a
signal that the abstraction is incomplete or the ownership boundary is wrong.

## Operational Knowledge To Capture

For every serious platform component, document:

- what the component owns;
- what it deliberately does not own;
- request and provisioning flows;
- state stores and consistency assumptions;
- logs that indicate expected progress;
- metrics that indicate health;
- common failure modes;
- how to stop, rollback, or drain bad changes;
- which teams depend on it;
- how a new engineer can safely make a small change.

This is especially important before on-call rotation expands. People need to
know where to look, what can go wrong, and which symptoms point to which part of
the system.

## Platform Diplomacy

Platform work changes how other teams work. Technical correctness is not enough.

Useful habits:

- explain the migration in terms of user outcomes and risk reduction;
- make the new path easier than the old path;
- keep compatibility where possible;
- provide examples for common service shapes;
- accept that product teams have local constraints the platform team may not
  see initially;
- use incidents and repeated support requests as product feedback.

## Mentoring And Knowledge Transfer

Teaching someone how a platform works is different from handing them answers.
The goal is to help them build a mental model of the system so they can debug
new situations.

Good mentoring questions:

- What do you think owns this state?
- Which part of the flow is control plane and which part is data plane?
- What would happen if this dependency were unavailable?
- Which metric would move first if this broke?
- How would you prove this change is safe before global rollout?

Give enough help to keep momentum, but leave enough room for the learner to make
the design and debugging connections themselves.

## Career Takeaway

Senior engineering depth shows up in how you maintain and explain systems:

- design small interfaces over complex internals;
- keep the operational model teachable;
- reduce repeated work for other teams;
- notice churn before it turns into permanent complexity;
- handle conflict and migration as part of the engineering work;
- turn hard-won production knowledge into docs, tests, tooling, and runbooks.
