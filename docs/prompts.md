# Prompts

Ready prompts for the moments that come up on every project. Copy one, fill the `<...>`, send. Each says when to reach for it.

## Starting

### Orient in a codebase

Before any change to code I did not write, or at the start of a session.

```text
Read the codebase before we change anything. Summarise: the stack, how to run it and its checks,
the folder layout and what lives where, where state lives, and the conventions the code follows
(naming, components, tests). Name the files you read. Point out anything that surprised you.
Do not change any file.
```

### Shape an idea

The first message of a new project. Runs phase 1 of `/agent-playbook:new-project`.

```text
I want to build <one sentence>. It is for <who>, who need to <job>. Grill me with
/mattpocock-skills:grill-with-docs until no branch of the design is left assumed. Ask in rounds,
a few questions at a time, and give your recommended answer with each question. Write the glossary
as terms are resolved. When we are done, write docs/product.md.
```

### Plan before code

After I have decided the approach, before the agent writes anything.

```text
I want to <goal>. My approach: <approach and state shape>. Give me a plan, not code: the files you
will touch or create, in order, what each change is, how we will verify each step, and anything
in the docs or ADRs it conflicts with. Keep it to the smallest change that does the job.
```

## Building

### One slice

Repeat per step of an agreed plan.

```text
Implement step <n> only: <what>. Done means: <acceptance criteria>. Touch only <files or folder>.
No new dependencies. Run pnpm check and show me the result. Then stop, and tell me what you would
check by hand.
```

### Write tests first

For pure logic and the seams a ticket names.

```text
Use /mattpocock-skills:tdd for <unit>. Cases: <happy path>, <edge cases: empty, one, many, invalid,
boundary>. Write the tests first and show them failing, then implement until they pass. Test
behaviour through the public interface, not implementation details.
```

### Debug

When something breaks.

```text
<symptom>. Expected: <expected>. Reproduce it first with a failing test or a script, and show me it
failing. Then find the cause; give me your top hypotheses before changing code. Fix the cause, not
the symptom, and show the reproduction passing.
```

## Reviewing

### Review my diff

Before committing a slice, and at the end of a ticket.

```text
Review the changes since <commit or branch> for correctness bugs first, then against CLAUDE.md and
.claude/rules/. For each finding: the file and line, what goes wrong, and a concrete input that
shows it. Skip style the formatter owns. Do not fix anything yet.
```

### Check a pull request

When a loop pull request is waiting.

```text
Check out <PR>. Run pnpm check. Walk me through what it changed and why, against its ticket in
.scratch/. Then list: anything the ticket asked for that is missing, any ticked acceptance box
you cannot verify, and anything outside the ticket's scope.
```

### Turn a review comment into a rule

When I catch the same kind of problem twice.

```text
I keep correcting <pattern>. Decide where the rule belongs, following docs/writing-rules.md:
a lint rule if lint can check it, else the narrowest .claude/rules/ file, else an ADR. Write it,
fix the existing code to follow it, and show me the rule and the diff.
```

## Deciding and recording

### Record a decision

When a choice with a real alternative has been made.

```text
Write an ADR for: <decision>. Alternatives we considered: <a>, <b>. Use
/mattpocock-skills:domain-modeling. State the decision as a fact, each alternative with the reason
it lost, and the consequences, including what would change if we reversed it.
```

### Wireframe a screen

Phase 3, one screen at a time.

```text
Draft the wireframe for <screen> in docs/wireframes.md, from docs/product.md: an ASCII layout,
then what sits in each region, then every state a user can see (empty, loading, error, overflow,
selected). Layout only, no colour or type.
```

### Hand off

When a session is long or I am stopping for the day.

```text
Use /mattpocock-skills:handoff. The next session will <goal>. Reference the docs, tickets and
commits instead of repeating them.
```

## Steering

Short lines for the middle of a session.

- `Stop. Plan first, no code.`
- `Smaller: only <file>.`
- `That is outside this task. Revert it.`
- `You added <package>. Remove it; use <what we have>.`
- `Show me the command output, not a summary.`
- `This failed twice. Stop and tell me what you know and what you would try next.`
- `Read <doc> and check your change against it.`
