# examplebuilder-consumer

Reference consumer candy for the build-time plugin-execution **builder** leg.

`examplebuilder-consumer` selects the out-of-tree external builder via
`external_builder: examplebuilder`. At image build, charly's build-path plugin
connect + `emitExternalBuilderStages` seam host-builds and connects
`candy/plugin-example-builder` out-of-process, invokes its `OpResolve`, and
splices the returned multi-stage block (pre-main-`FROM`) plus the
`COPY --from` artifacts (post-main-`FROM`) into the generated Containerfile —
baking `/opt/examplebuilder-artifact` into the image. Compose it **with**
`candy/plugin-example-builder` (which provides the `examplebuilder` builder); the
runtime check proves the build-resolve ran.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `examplebuilder-consumer` |
| External builder | `examplebuilder` (provided by `candy/plugin-example-builder`) |
| Artifact | `/opt/examplebuilder-artifact` |
| Service / port | none |

This is a **reference/fixture** candy: it exists to exercise and demonstrate the
build-time external-builder seam, not to be composed into production boxes.

## How to use it

Compose it together with the plugin candy that provides the builder:

```yaml
my-builder-demo:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-examplebuilder-consumer:v2026.239.1621'
      - '@github.com/opencharly/plugin-example-builder/candy/plugin-example-builder:v2026.239.1643'
```

After the image is built, the runtime check proves the resolve ran:

```bash
test -f /opt/examplebuilder-artifact
```

## Layout

- `charly.yml` — the `examplebuilder-consumer:` candy entity: the
  `external_builder:` selection and the runtime `check:` step.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Plugin provider: `candy/plugin-example-builder`
- Sibling fixtures: `/charly-internals:plugin` — the plugin authoring reference
  covering the builder provider class
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
