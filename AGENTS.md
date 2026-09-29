# AGENTS.md — layer-examplebuilder-consumer

Standalone reference/fixture candy repo for the build-time plugin-execution
**builder** leg. The candy lives in `charly.yml` at the repo root: the
`external_builder: examplebuilder` selection and the runtime `check:` step that
proves the build-resolve ran. It carries **no `skill:` entity**, so no owning
`/charly-<family>:<name>` skill is projected into the marketplace corpus.

Canonical files:

- `charly.yml` — the `examplebuilder-consumer:` candy entity (no `skill:`
  entity present).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the closest owning skill: the plugin model, the
  provider classes (including the builder provider this fixture exercises), the
  per-plugin CUE schema contract, and the loader. Load before editing or
  troubleshooting the fixture.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the `external_builder:` selection, package
  sections). Load before editing any entity field or plan step.

There is no dedicated `/charly-*:examplebuilder-consumer` owning skill — this
repo's candy carries no `skill:` entity. The gap is recorded against
`opencharly/opencharly#291`; when one is authored, add it here.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` step is the functional evidence:
  `examplebuilder-artifact-present` asserts `/opt/examplebuilder-artifact` exists
  in the built image (runtime context), proving the build-path plugin connect +
  `OpResolve` splice ran.
- The fixture only proves anything when composed WITH
  `candy/plugin-example-builder`, which provides the `examplebuilder` builder.

## Modify this repo

- Edit the `examplebuilder-consumer:` candy entity in `charly.yml`. Keep the
  `external_builder:` name aligned with the builder provider the plugin candy
  declares, and keep the runtime `check:` asserting the spliced artifact.
- This is a reference/fixture candy — a change here is a change to the documented
  external-builder example, so keep it minimal and observable.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
