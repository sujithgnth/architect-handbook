# Prompt Engineering

Prompt engineering is the practice of giving an AI model the role, context,
instructions, evidence, examples, and output contract it needs to produce a
reliable result. Treat prompts as small product or workflow designs, not as
magic phrases.

Source note:

- [Prompting 101 | Code w/ Claude](https://www.youtube.com/watch?v=ysPbXH0LpIE)
  by Anthropic

## Core Lesson

Start with a small prompt, test it against a real task, inspect the failure, and
add only the missing structure. Good prompts are built iteratively from observed
model behavior.

The demo uses a Swedish car insurance claim workflow: the model receives an
accident form and a hand-drawn sketch, extracts objective facts, and decides
which vehicle is primarily at fault. The early prompt fails because the model is
missing domain context and task order. Later versions improve by adding stable
background knowledge, explicit instructions, evidence requirements, and a fixed
output shape.

## Prompt Structure

Use this order for serious one-shot or API prompts:

1. Role, task, and success criteria.
2. Stable context and domain rules.
3. Dynamic input or retrieved content.
4. Ordered task instructions.
5. Examples or tricky cases.
6. Confidence, evidence, and hallucination rules.
7. Output format.

Put stable, reusable knowledge in the system prompt. Put changing request data
in the user prompt. In the insurance example, the form structure belongs in the
system prompt because it does not change; the specific accident form and sketch
belong in the user prompt because they change per claim.

## Useful Prompt Blocks

Use descriptive sections or XML-style tags when a prompt mixes different kinds
of information.

```text
<role>
You are a claims analyst reviewing vehicle accident reports.
</role>

<task>
Extract objective facts from the form and sketch, then decide whether Vehicle A
or Vehicle B is primarily at fault.
</task>

<instructions>
1. Read the form first.
2. Record checked items and visible facts.
3. Read the sketch second.
4. Compare the sketch against the form facts.
5. Give a verdict only if the evidence is clear.
</instructions>

<output_format>
Return only:
- facts
- evidence
- confidence
- final verdict
</output_format>
```

The exact tag names are less important than consistency and clear separation of
context, task, examples, and output contract.

## Order Matters

Tell the model what to inspect first when the order affects accuracy. In the
demo, reading the structured form before the sketch reduced misleading visual
interpretations. The general rule is:

- objective facts first;
- ambiguous interpretation second;
- final judgment last.

This is useful for code review, incident analysis, architecture review,
interview answer review, and any workflow where evidence should come before
opinion.

## Evidence And Confidence Rules

For judgment tasks, require the model to ground claims in evidence.

Useful rules:

- State uncertainty instead of guessing.
- Use only facts present in the input or trusted context.
- Cite which input supports the conclusion.
- Separate factual extraction from interpretation.
- Give a verdict only when confidence is high enough.

This makes the result more auditable and easier to debug.

## Examples

Examples are most useful when instructions cannot fully describe the desired
behavior. Keep a small library of representative and tricky cases.

Good examples should:

- match the real task closely;
- cover edge cases, not only the happy path;
- show the exact output shape;
- avoid teaching accidental formatting or reasoning habits.

Use examples to teach style, domain judgment, and output structure.

## Output Contracts

Production prompts need a predictable output contract. Use JSON, XML, Markdown
sections, or another parseable shape depending on the downstream system.

For early development, include diagnostic sections such as extracted facts and
evidence. For production, keep only the parts the downstream system needs.

## Practical Checklist

Before using an important prompt, check:

- Is the role and task explicit?
- Is stable context separated from dynamic input?
- Are instructions ordered?
- Are examples included only when they add real signal?
- Does the prompt say what to do when evidence is missing?
- Is the output shape easy to parse or review?
- Can a human with minimal context follow the prompt?
- Has it been tested on at least a few real or realistic cases?

## Common Mistakes

- Starting with a giant prompt before seeing how the small prompt fails.
- Mixing instructions, examples, and user data in one paragraph.
- Asking for a judgment before extracting facts.
- Forgetting confidence and uncertainty rules.
- Optimizing for a pretty answer instead of a reliable output contract.
- Treating the first good result as proof that the prompt is robust.

## References

- [Anthropic: Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- [Prompting 101 video transcript excerpt on Glasp](https://glasp.co/youtube/ysPbXH0LpIE)
- [Code w/ Claude 2025 notes](https://www.caiying.me/posts/code-with-claude-2025-notes/)
