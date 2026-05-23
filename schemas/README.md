# JSON Schema artifacts

| File | Status | Spec link |
| --- | --- | --- |
| [`rde-review-output.phase2.schema.json`](rde-review-output.phase2.schema.json) | **Normative** (Phase 2 validation profile, SLS-9) | [`phase2-interchange-hardening.md`](../docs/phase2-interchange-hardening.md) |
| [`rde-assessment-storage.v0.1.draft.schema.json`](rde-assessment-storage.v0.1.draft.schema.json) | **Informative draft** | [`rde-assessment-storage-increment.md`](../docs/rde-assessment-storage-increment.md) · [#47](https://github.com/zyx-corporation/kotonoha-spec/issues/47) |

## Examples

- [`examples/rde-assessment-storage.minimal.json`](examples/rde-assessment-storage.minimal.json) — empty-category RDE payload + M2 metadata (draft validation smoke).

## Local validation (optional)

```bash
# requires a JSON Schema validator (e.g. npx ajv-cli)
npx --yes ajv-cli validate -s schemas/rde-assessment-storage.v0.1.draft.schema.json -d schemas/examples/rde-assessment-storage.minimal.json
```
