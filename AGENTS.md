# AGENTS.md - OITC Access Control System Documentation

## Repository Overview

This is the architecture documentation repository for the OITC Access Control System (ACS). The repository contains documentation files, architecture diagrams, and related assets. There is no executable code in this repository.

## Project Structure

```
acs.documentation/
├── README.md           # Project overview
├── LICENSE             # License file
├── media/              # Images and diagrams
│   ├── acs-pushover-icon.png
│   └── 2026-03-21-microservice-architecture.png
└── docs/               # Documentation files (if any)
```

## Build and Development Commands

Since this is a documentation-only repository, there are no build, lint, or test commands.

### Documentation Preview (if using a static site generator)

If documentation is moved to a static site generator in the future:

- **Preview locally**: Depends on the generator (e.g., `npm run dev`, `hugo server`, etc.)
- **Build**: `npm run build` or equivalent
- **Deploy**: Manual deployment to hosting platform

## Code Style Guidelines

This repository contains no executable code. All guidelines below apply to documentation files.

### Markdown Style

- Use ATX-style headers (`#`, `##`, `###`) rather than Setext style
- Use fenced code blocks with language identifiers: ```` ```language ````
- Use tables for structured data when appropriate
- Keep lines reasonably sized (max ~120 characters where practical)
- Use consistent heading hierarchy (don't skip levels)

### File Naming

- Use lowercase with hyphens for documentation files: `architecture-overview.md`
- Use descriptive names: `microservice-design.md` not `doc1.md`
- Group related docs in directories

### Diagrams

- Store diagram source files (e.g., Mermaid, PlantUML, draw.io) in `media/`
- Export final diagrams to PNG/SVG for markdown inclusion
- Include diagram source in version control when practical

### Version Control

- Commit messages should be clear and descriptive
- Follow conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`
- Review changes before committing

## Error Handling

Since there is no code, error handling does not apply to this repository.

If code is added in the future, follow these principles:
- Validate all inputs
- Provide clear error messages
- Log errors appropriately
- Never expose sensitive information in error messages

## Naming Conventions

### Documentation Files

- Use kebab-case: `access-control-flow.md`
- Be descriptive: `api-authentication-guide.md` not `api.md`

### Branches

- Use meaningful names: `feature/add-authentication-docs`
- Use prefixes: `feature/`, `fix/`, `docs/`

### Commits

- Use imperative mood: "Add documentation" not "Added documentation"
- Keep subject line under 72 characters
- Reference issues when applicable

## General Guidelines for Documentation

1. **Clarity**: Write in clear, concise language
2. **Accuracy**: Verify technical details
3. **Completeness**: Include necessary context and prerequisites
4. **Accessibility**: Use descriptive link text, not "click here"
5. **Maintenance**: Keep docs updated with code changes

## Existing Rules

There are no existing Cursor rules (`.cursor/rules/`, `.cursorrules`) or Copilot rules (`.github/copilot-instructions.md`) in this repository.

## Future Considerations

If this repository evolves to include code:

- Add appropriate build/lint/test commands to this file
- Include language-specific style guides
- Add CI/CD pipeline configuration
- Document deployment procedures

## Contact

For questions about this documentation, contact the OITC development team.
