# AI Code Review & Debugging Prompts

A practical collection of copy-paste prompts that make AI assistants useful for real code work: reviews that find bugs, debugging that finds causes, refactors that don't break behaviour.

Works with any assistant: ChatGPT, Claude, Gemini, local models, or [MadGPT](https://madgpt.io).

**Why these work:** most bad AI answers come from yes/no questions, missing context, and long polluted chats. Every prompt here fixes one of those three.

---

## Contents

- [Code review](#code-review)
- [Debugging](#debugging)
- [Refactoring](#refactoring)
- [Tests](#tests)
- [Security review of your own code](#security-review-of-your-own-code)
- [Planning and agents](#planning-and-agents)
- [Context management](#context-management)
- [General prompting rules](#general-prompting-rules)
- [Contributing](#contributing)

---

## Code review

**1. Failure modes instead of approval**
```
What are the 3 most likely ways this code breaks in production?
Rank them by impact and give a concrete fix for each.
```

**2. The 3 a.m. reviewer**
```
Review this as a senior engineer who has been paged at 3 a.m. by this
service twice this month. Focus on timeouts, retries, error handling,
logging and resource leaks. Ignore style.
```

**3. Diff review**
```
Here is a diff. For each change, tell me:
- what behaviour changes,
- what could break that is not covered by the diff,
- whether you would approve it, and why.
```

**4. Edge cases only**
```
List every input or state this function does not handle correctly.
For each, give a minimal example that reproduces the problem.
```

**5. Readability pass (after correctness)**
```
The logic is correct. Suggest at most 5 changes that make this easier to
read for a new team member. Do not change behaviour.
```

## Debugging

**6. The three things to paste**

Before asking, always include:
1. The exact error, copied, not paraphrased.
2. The function that throws it, not the whole repo.
3. What you expected to happen.

```
Error (exact):
<paste>

Code:
<paste the function>

Expected: <what should happen>
Actual: <what happens>

Find the root cause first. Then give the smallest fix.
```

**7. Hypotheses before fixes**
```
Give me 3 hypotheses for why this fails, ordered by likelihood.
For each, tell me one check that would confirm or rule it out.
Do not write a fix yet.
```

**8. Screenshot debugging**
```
This is a screenshot of the error and stack trace. Read it, tell me which
frame is the real origin of the bug, and why.
```

**9. Works locally, fails in CI/prod**
```
This passes locally and fails in <CI / production>. List the environment
differences that could cause this, most likely first, with a way to test each.
```

## Refactoring

**10. Behaviour-preserving refactor**
```
Refactor this to <goal>. Constraints: identical external behaviour,
same public API, no new dependencies. List every behaviour you preserved
and how you checked it.
```

**11. Legacy code, step by step**
```
Propose a refactor of this legacy module as a sequence of small steps.
Each step must leave the code working and be reviewable on its own.
```

**12. Explain before you change**
```
Before changing anything, explain what this code does, line by line,
and point out anything that looks intentional but strange.
```

## Tests

**13. Tests that would have caught the bug**
```
Here is a bug and its fix. Write the test that fails before the fix and
passes after it. Then suggest 2 related tests that are missing.
```

**14. Test plan**
```
Write a test plan for this function: happy path, edge cases, error cases.
Mark which ones need mocks and why.
```

## Security review of your own code

**15. Ranked vulnerability review**
```
This is code from my own project. Review it for security issues
(injection, auth, secrets, unsafe deserialization, SSRF, path traversal).
Rank findings by impact and give a patch for each.
```

**16. Input trust audit**
```
List every place where this code trusts input from outside the process
(user input, files, env vars, network). For each, say how it is validated
or that it is not.
```

## Planning and agents

**17. Plan first**
```
Before writing any code, list the files you would change and why.
Wait for my OK.
```

**18. Make it explain its own diff**
```
Explain your diff line by line. Point out anything you are unsure about.
```
If the model can't explain a change clearly, the change is probably wrong. This catches hallucinated APIs fast.

**19. Scope guard for coding agents**
```
Only touch files related to <task>. If you think another file needs a change,
stop and ask instead of editing it.
```

## Context management

**20. The 5-line handoff**

When a long chat starts repeating itself or forgetting decisions, ask:
```
Summarize this conversation in 5 lines: goal, constraints, decisions made,
current state, next step.
```
Check it, open a new chat, paste the summary plus the current version of the file.

**21. One chat per task**

Don't debug auth and design a landing page in the same thread. Old context competes with new context.

## General prompting rules

**22. Ask for the result, not for permission.**
"Could you maybe help me write…" gets a hedged answer. "Write X. Output only the code." gets working output.

**23. Specify the format.**
"Give 3 options and pick the best" beats "what do you think?".

**24. Ask for a position.**
"Give your conclusion in one paragraph, then the arguments" removes "on one hand, on the other hand".

**25. Give the context a human would need.**
"I'm a developer, this is my own project, here is the code" gets better answers than a bare question.

---

## Contributing

Pull requests are welcome. A good prompt here:
- solves one specific problem,
- works with any assistant,
- comes with one line on why it works.

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/): use, copy and adapt freely, no attribution needed.

---

Maintained by the team behind [MadGPT](https://madgpt.io), an AI workspace with 7 models in one chat, an opt-in Uncensored model and a VS Code agent. More tips on [DEV](https://dev.to/madgpt).
