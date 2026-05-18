---
name: grill
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time, waiting for feedback on each question before continuing.

If a question can be answered by exploring the codebase, explore the codebase instead.

## Paperclip Context

When running inside a Paperclip instance, questions and answers happen through issue comments. Post your questions as comments on the issue. When a decision is resolved, update the issue `plan` document with the outcome.

If a design question requires human approval before proceeding, use a `request_confirmation` interaction tied to the latest plan revision and set the issue to `in_review`.