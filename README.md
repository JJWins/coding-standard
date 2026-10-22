# Coding Standard

This repository maintains our shared rules and coding standards for AI coding assistants.

Instead of repeating coding conventions, architectural boundaries, and security checks in every chat prompt, we keep them here as reusable rule files that can be dropped into any project.

## What This Repo Covers

- **Core Standards**: General code style, naming conventions, error handling, and git workflow.
- **Security**: Guardrails for secret management, input validation, safe data access, and logging.
- **Stack Conventions**: Guidelines for the languages, frameworks, and tools we use.
- **Project Structure**: Templates for documenting a project's layout, layers, and shared utilities.
- **Quality & Testing**: Expectations for tests, API design, and code maintainability.

## Using These Rules

Depending on the tool your project uses:

- **Cursor**: Copy the relevant rule files into your project's `.cursor/rules/` folder. See [`.cursor/rules/README.md`](./.cursor/rules/README.md) for the folder layout and instructions on adding new rules.
- **Agent Instructions (`AGENTS.md`)**: Reference or include the core rules in the root `AGENTS.md` file of your repository.
- **Other Assistants**: Adapt the markdown rules into your assistant's instruction file.

## Security

Every project using these rules should include the core security guidelines by default:
- Generated code must never contain hardcoded secrets, unvalidated inputs, or unsafe queries.
- Rule files themselves must never contain real credentials, internal endpoints, or sensitive company data. Use generic placeholders in all documentation and examples.

## Contributing

When adding or updating a rule:
1. Keep each file focused on one area (global, stack, or project).
2. Write clear, practical points—focus on what our team actually does rather than generic textbook advice.
3. Keep rules brief so they don't overload the assistant's context window.
4. Open a pull request with a short note explaining why the rule is needed.
