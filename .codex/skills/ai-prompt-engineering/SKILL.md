---
name: ai-prompt-engineering
description: Use when writing, reviewing, debugging, or improving prompts for Claude or other AI systems, especially prompts that need clear roles, context, examples, evidence rules, structured inputs, confidence handling, output formats, or production-ready prompt workflows.
---

# AI Prompt Engineering

Use this skill to turn vague AI instructions into reliable prompts. Prefer an
iterative workflow: start small, test against real examples, inspect failures,
then add only the missing structure.

## Prompt Anatomy

For serious one-shot, API, or workflow prompts, include:

1. Role, task, and success criteria.
2. Stable context and domain rules.
3. Dynamic input or retrieved content.
4. Ordered task instructions.
5. Examples or tricky cases when needed.
6. Confidence, evidence, and hallucination rules.
7. Output format.

Put stable, reusable knowledge in the system prompt. Put changing request data
in the user prompt.

## Structure

Use clear Markdown sections or XML-style tags when the prompt mixes context,
instructions, examples, and variable inputs. Tag names should be descriptive and
consistent, such as `role`, `context`, `input`, `instructions`, `examples`,
`constraints`, and `output_format`.

## Evidence First

For judgment tasks, force the order:

1. Extract objective facts.
2. Inspect ambiguous evidence.
3. Compare against rules or examples.
4. State uncertainty if evidence is weak.
5. Give the final answer only after the evidence pass.

This reduces hallucination and makes the output easier to audit.

## Examples

Use examples when rules are hard to describe precisely. Keep examples close to
the real task, include edge cases, and show the expected output shape. Avoid
overfitting the prompt to one example.

## Output Contracts

Specify the output shape when downstream systems or reviews depend on it. Use
JSON, XML, Markdown sections, or a compact schema. During development, include
diagnostic fields such as facts, evidence, confidence, and final answer. In
production, keep only what the consumer needs.

## Review Checklist

- Can a human with little context follow the prompt?
- Is stable context separated from dynamic input?
- Are instructions ordered where order matters?
- Does the prompt define what to do when evidence is missing?
- Are examples relevant and diverse?
- Is the output contract explicit?
- Has the prompt been tested on realistic success and failure cases?

## Related Handbook Notes

- `coding-practices/prompting/README.md`
