# ARCHITECTURE.md

The architecture document for this repository, following the
[architecture.md](https://architecture.md) schema — built so an agent (or a
new colleague) can comprehend the codebase from this file alone, and so the
architectural principles in the `software-architecture` and
`software-development` skills are visible in how this repo actually works.
The Node/TypeScript conventions from
[node-skeleton](https://github.com/99linesofcode/node-skeleton) are baked in
(ARCHITECTURE.md there is the canonical statement). Fill every section;
update it in the same change that alters the architecture it describes.

## 1. Project Structure

Module-first, lowercase — the top level screams the domain. The
architecture axis is carried by file-name role suffixes (`Action`,
`Adapter`, `Port`, `Mapper`), not layer folders. The composition root is
`src/main.ts` (apps) or `src/index.ts` (libraries) — at the src root, above
the modules. Tests mirror the tree. State where the logic lives: which
concerns sit in actions, which in pure calculations, which in domain
services — and why.

```
[Project Root]/
├── src/
│   ├── <module>/         # one folder per bounded concept, lowercase
│   ├── shared/           # the kernel: canonical DTOs, pure helpers — imports from no module
│   └── main.ts           # the composition root — wires the providers
├── tests/                # mirrors src/
├── docs/                 # developer manual (flows as sequence diagrams)
├── eslint.config.js      # boundary enforcement lives here (eslint-plugin-boundaries)
└── package.json
```

## 2. High-Level System Diagram

```
[User] <--> [System] <--> [External Service]
```

## 3. Core Components

For each module: name, primary responsibility, key technologies, deployment
target.

### Ports & adapters

List each port the core owns: the core NEED it serves (never the tool's
API it wraps), and the adapter(s) implementing it. If a component's logic
reaches around a port, that is a defect — document it as debt or fix it.

## 4. Data Stores

Name, type, purpose, key schemas/collections (names only).

## 5. External Integrations / APIs

Name, purpose, integration method (REST, SDK, webhook). Note which port
each integration sits behind.

## 6. Deployment & Infrastructure

Cloud provider, key services, CI/CD pipeline (GitHub Actions), monitoring
and logging.

## 7. Security Considerations

Authentication, authorization, encryption, secret handling, tooling.

## 8. Development & Testing Environment

Local setup (the devshell: `direnv allow`), testing framework (vitest),
code quality tools (eslint with boundary rules, prettier, tsc) — including
the mechanical gates and what each gate makes impossible.

## 9. Future Considerations / Roadmap

Known architectural debt, planned major changes — and the **deliberate
non-goals**: what the lean guardrail excluded, and why. A non-goal recorded
here is a decision; one that isn't recorded gets re-proposed every quarter.

## 10. Project Identification

Project Name: [name]

Repository URL: [url]

Primary Contact/Team: [owner]

Date of Last Update: [YYYY-MM-DD]

## 11. Glossary / Acronyms

Project-specific terms and acronyms, defined.

## 12. Conventions & Boundaries

Enforced by `eslint-plugin-boundaries` (elements = the module folders) and
the `lint:boundaries` vocabulary grep — a naming standard without a gate
erodes one change at a time:

- **Folder structure**: module-first, lowercase; the path locates the
  module, the name locates the role.
- **File naming**: PascalCase classes with role suffixes; camelCase pure
  functions, one per file.
- **Entry point**: `src/main.ts` / `src/index.ts` — above the modules,
  never inside one.
- **Dependency matrix**: the shared kernel imports from no module; provider
  modules never import each other; neutral modules consume the kernel and
  the ports that live in it — never a provider adapter directly; the
  composition root wires everything; no circular module dependencies.
- **Provider neutrality**: provider names appear only in provider modules
  and the composition root; shared and cross-cutting vocabulary is neutral.
- **Canonical DTOs**: one canonical shape per domain concept, owned by the
  core; diff/merge logic operates on canonical fields only. A DTO mimicking
  a provider's structure is a provider shape, whatever its file name.
- **Documentation surfaces**: WHY comments at the change site; the
  developer manual updated when a flow changes; the behavioral contract
  amended only by the owner.
