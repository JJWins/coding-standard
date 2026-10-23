# Coding Standard

This repository maintains our shared rules and coding standards for AI coding assistants.

Instead of repeating coding conventions, architectural boundaries, and security checks in every chat prompt, we keep them here as reusable rule files that can be dropped into any project.

## Rule Categories

Rules live under [`.cursor/rules/`](./.cursor/rules/README.md) and are organized by scope:

- **`core/`**: Global engineering principles, security guardrails, error handling, and logging standards that apply across every project.
- **`quality/`**: Pull request review gates (`Blocking` vs. `Standard`), testing expectations, and UI/accessibility standards.
- **`stacks/`**: Conventions for specific languages, frameworks, and data layers, loaded only when working on matching file types.
- **`project/`**: Rules tailored to an individual product or repository's architecture, shared utilities, and domain workflows.

## Using These Rules

Depending on the tool your project uses:

- **Cursor**: Copy the `core/`, `quality/`, and relevant `stacks/` rules into your project's `.cursor/rules/` folder, then add your repository's rules under `.cursor/rules/project/`. See [`.cursor/rules/README.md`](./.cursor/rules/README.md) for the rule file format and frontmatter options.
- **Agent Instructions (`AGENTS.md`)**: Reference or include these rules in the root `AGENTS.md` file of your repository.
- **Other Assistants**: Adapt the markdown rules into your assistant's instruction file.

## Adding Project-Specific Rules

Shared rules cover general engineering and stack practices, but each project should define its own rules under `.cursor/rules/project/` (or in its dedicated product branch) so the assistant understands how that codebase works:

1. **Architecture & Layer Boundaries**: Document the folder layout, module responsibilities, and dependency direction (for example, which modules are shared core libraries vs. feature modules).
2. **Existing Utilities & Base Classes**: List internal services, base entities, auth helpers, request clients, and shared UI components so the assistant reuses them instead of creating duplicates.
3. **Domain & Operational Workflows**: Capture project-specific requirements such as resource authorization checks, multi-tenant configuration parameters, search or cache synchronization, and localization files that must stay in sync.
4. **Right-Sized Scope**:
   - Set `alwaysApply: true` for high-level project architecture and cross-cutting domain rules.
   - Set `alwaysApply: false` with `globs` for rules that only apply to a specific module, service, or folder.
   - Keep stack- or project-specific rules out of `core/` and `quality/` so shared rules remain portable across repositories.

## Security

Every project using these rules should include the core security guidelines by default:

- Generated code must never contain hardcoded secrets, unvalidated inputs, or unsafe queries.
- Rule files themselves must never contain real credentials, internal endpoints, or sensitive company data. Use generic placeholders in all documentation and examples.

## Contributing

When adding or updating a rule:

1. Keep each file focused on a single topic (`core`, `quality`, `stacks`, or `project`) and under ~100 lines.
2. Write short, direct points—focus on what our team actually enforces rather than generic textbook advice.
3. Open a pull request with a short note explaining why the rule is needed.
