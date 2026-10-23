# Rules Guide

This directory holds the rule files used to guide AI coding assistants in our projects. The goal is to keep the assistant aligned with how our team writes, structures, and secures code without repeating ourselves in every prompt.

## Using Rules in the Cursor IDE

Project rules are read from `.cursor/rules/` in the open workspace and included in Agent chat. Cursor Tab and inline edit do not use them. Only `.mdc` files are loaded; this README is documentation and is not sent to the Agent.

1. Copy the `.cursor/rules/` folder from the branch that matches the project into the target project. `main` contains the shared rules. A product branch, such as `Product/Fibi`, also contains that project's rules.
2. Open the project folder as the Cursor workspace. Rules in a parent folder or another repository are not included.
3. Open **Customize** in the sidebar, then **Rules**, and confirm each copied rule appears with the expected type: Always Apply, Apply to Specific Files, Apply Intelligently, or Apply Manually.
4. Start an Agent chat. Cursor includes each rule according to its frontmatter:
   - `alwaysApply: true` is included in every Agent chat.
   - `alwaysApply: false` with `globs` is included when a matching file is open, attached, or being edited.
   - `alwaysApply: false` with a `description` and no `globs` is included when the Agent decides the description matches the request.
   - A rule with neither `description` nor `globs` is included only when you mention it.
5. To force a rule into the current chat, type `@` in the Agent input and select it, for example `@fibi40-java-review-policy`.
6. If a file-scoped rule is missing from the response, attach the matching source file or mention the rule with `@`, then check **Customize → Rules** to confirm Cursor discovered the file.

## Folder Structure

Rules are split into folders based on where they apply:

```text
.cursor/rules/
├── core/          # Global rules that apply everywhere (coding style, security, error handling)
├── stacks/        # Rules for specific languages, frameworks, or data layers
├── quality/       # Standards for testing, code reviews, UI accessibility, and API contracts
└── project/       # Rules specific to a single project's architecture and domain
```

## Rule File Format

Each rule is a `.mdc` file with a short frontmatter header at the top:

```markdown
---
description: What this rule covers and when it should be used
globs: <file-pattern>
alwaysApply: false
---

# Rule Title

- Clear, direct guidelines on what to follow and what to avoid.
```

- `description`: A short summary so the assistant knows what the file is for.
- `globs`: File or folder patterns where the rule applies. Leave this out if `alwaysApply` is `true`.
- `alwaysApply`: Set to `true` for universal rules in `core/` and repo-wide `quality/` gates (such as PR review and testing standards). Set to `false` (with `globs`) for stack-specific or file-scoped rules (such as frontend UI/accessibility checks).

## Adding Stack Rules (`stacks/`)

Stack rules cover conventions for a specific language, framework, or tool.

- Scope the rule using `globs` so it only loads when working on relevant files, and set `alwaysApply: false`.
- Skip basic language features the assistant already knows. Focus on our team's preferred libraries, patterns, and conventions.
- Clearly state which approaches we prefer and which ones to avoid.

## Adding Project Rules (`project/`)

Project rules explain how an individual repository is put together so new code lands in the right place and reuses existing code.

- **Folder layout**: Explain where different layers, modules, and shared components belong.
- **Existing utilities**: Point out internal helpers (logging, error handling, request clients, shared components) so the assistant uses them instead of writing new ones from scratch.
- **Domain rules**: Note any business rules, naming conventions, or architectural boundaries that must be respected.
- Set `alwaysApply: true` for core project structure, or scope it to a specific module path using `globs`.

## Security Guidelines

Security rules live in `core/` with `alwaysApply: true` so they are active in every session.

### In Generated Code
- **Secrets**: Never hardcode credentials, keys, tokens, or connection strings. Always read them from environment variables or a secret manager.
- **Input handling**: Validate and sanitize all external inputs before processing them.
- **Queries & commands**: Use parameterized queries or safe library methods. Never build queries or system commands by concatenating raw input.
- **Access control**: Enforce authentication and permission checks on all protected operations.
- **Logging & errors**: Do not leak stack traces, internal details, credentials, or personal data in logs or error responses.

### In Rule Files
- Never put real credentials, internal URLs, private keys, or sensitive project data inside rule files. Always use generic placeholders.

## Keeping Rules Useful

- Keep each file focused on a single topic.
- Write short, direct points rather than long paragraphs.
- Keep files under ~100 lines so they stay easy to maintain and don't crowd the assistant's context.
