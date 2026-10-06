# ARCHITECTURE.md

The architecture document for this repository, following the
[architecture.md](https://architecture.md) schema, with the Node/TypeScript
conventions from [node-skeleton](https://github.com/99linesofcode/node-skeleton)
baked in (ARCHITECTURE.md there is the canonical statement). Fill every
section; update it in the same change that alters the architecture.

## 1. Project Structure

Module-first, lowercase — the top level screams the domain. The
architecture axis is carried by file-name role suffixes (`Action`,
`Adapter`, `Port`, `Mapper`), not layer folders. The composition root is
`src/main.ts` (apps) or `src/index.ts` (libraries) — at the src root, above
the modules. Tests mirror the tree.

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

## 4. Data Stores

Name, type, purpose, key schemas/collections (names only).

## 5. External Integrations / APIs

Name, purpose, integration method (REST, SDK, webhook).

## 6. Deployment & Infrastructure

Cloud provider, key services, CI/CD pipeline (GitHub Actions), monitoring
and logging.

## 7. Security Considerations

Authentication, authorization, encryption, secret handling, tooling.

## 8. Development & Testing Environment

Local setup (the devshell: `direnv allow`), testing framework (vitest),
code quality tools (eslint with boundary rules, prettier, tsc).

## 9. Future Considerations / Roadmap

Known architectural debt, planned major changes.

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
- **Documentation surfaces**: WHY comments at the change site; the
  developer manual updated when a flow changes; the behavioral contract
  amended only by the owner.
