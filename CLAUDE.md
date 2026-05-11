# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Bilingual Comments (EN + ZH)

**Every function gets bilingual comments. Every new file gets a bilingual header.**

### Functions

Each function (and method) must carry both an English and a Chinese comment, English first, Chinese second, placed immediately above the signature. Use the language's native comment syntax.

```js
// EN: Validate user input and return the normalized form.
// ZH: 校验用户输入并返回规范化后的结果。
function normalize(input) { ... }
```

```python
def normalize(input):
    # EN: Validate user input and return the normalized form.
    # ZH: 校验用户输入并返回规范化后的结果。
    ...
```

Rules:
- One line each is enough — describe *what* and *why*, not the obvious *how*.
- Keep both versions in sync. If you update one, update the other in the same edit.
- Applies to new functions and to existing functions you modify. Do not touch unmodified functions just to add comments (see Rule 3).

### New files

Every newly created file must start with a bilingual header describing its purpose / responsibility. Use the file's comment syntax; place it before any imports.

```js
// EN: HTTP request validators shared across the API layer.
// ZH: API 层共用的 HTTP 请求校验器集合。
```

```python
"""
EN: HTTP request validators shared across the API layer.
ZH: API 层共用的 HTTP 请求校验器集合。
"""
```

Rules:
- One sentence per language is the target — state the file's role, not its contents.
- Required only for files **you create**. Do not retrofit headers into pre-existing files unless asked.
- If you rename or significantly repurpose a file, update the header.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
