# Website Documentation Generator Pack

**Purpose:** A reusable source pack for Hermes or another coding agent to generate a complete, project-specific website documentation folder and ZIP from a user's request, attachments, screenshots, repository and constraints.

## What this pack contains

- `docs/` — 35 individual source-format documents, preserving the supplied Website Plan Format headings and coverage.
- `prompts/MASTER-GENERATION-INSTRUCTIONS.md` — the primary instructions the AI must follow before generating a website plan.
- `prompts/QUICK-START-PROMPT.md` — a short prompt to start a new website documentation request.
- `DOC-INDEX.md` — canonical document order and cross-reference map.
- `skills/HERMES-SKILLS-AND-DOC-MAP.md` — recommended specialist skills by document and work stage.
- `templates/` — intake, decision register, risk register, project settings and acceptance templates.
- `reference/` — the original supplied source file and original build-guide / changelog excerpts.

## How to use this folder in Hermes

1. Extract the ZIP into a stable project or workspace folder.
2. Open that folder as the working context / repository in Hermes.
3. Start a new session and paste `prompts/QUICK-START-PROMPT.md` or ask Hermes to read `prompts/MASTER-GENERATION-INSTRUCTIONS.md`.
4. Describe the website you want, attach any company profile, existing docs, designs, screenshots, assets, frame folders or repository files.
5. Tell Hermes which output you want: all 35 documents, a smaller scope-approved set, or both a master Markdown file and separate files.
6. Hermes must read all applicable files in this pack before generating. It must inspect the actual project materials; it must not assume the old example's stack or scope.
7. Ask Hermes to place the generated project folder in the workspace and create a ZIP. Verify the ZIP contains every file listed in its own index.

## Important rule

These documents originate from a scroll-driven video website plan. They are a format and coverage reference, not a universal architecture mandate. For a different website, Hermes must preserve useful document structures while adapting, marking not-applicable or deferring unnecessary modules. Do not automatically add a database, authentication, admin panel, payment gateway, analytics tracker or paid API to a simple company website.

## Recommended generated output structure

```text
<PROJECT-NAME>_Website_Documentation/
├── README.md
├── MASTER-DOCUMENTATION.md
├── DOC-INDEX.md
├── CHANGELOG.md
├── PROJECT-BRIEF.md
├── DECISION-REGISTER.md
├── SKILLS-AND-AGENT-MAP.md
├── docs/
│   ├── 01-PRD.md
│   ├── 02-TRD.md
│   ├── ...
│   └── 35-USER-GUIDE-FAQ-TROUBLESHOOTING.md
├── templates/
├── prompts/
│   └── IMPLEMENTATION-HANDOFF-PROMPT.md
└── delivery/
    └── README-AND-ZIP-CHECKLIST.md
```

## File count

There are exactly 35 files in `docs/`. Supporting prompts, index, README, skill map, reference files and reusable templates are separate and do not change the 35-document count.
