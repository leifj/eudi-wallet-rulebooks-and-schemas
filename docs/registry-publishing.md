# Publishing on registry.siros.org

The SIROS Credential Registry builds its catalogue of attestations with [registry-cli](https://github.com/sirosfoundation/registry-cli). This repository follows the layout of registry-cli's `rulebook-catalog` plugin, so one source entry publishes every attestation type in it.

## Source entry

```yaml
sources:
  - url: "git:https://github.com/EWC-consortium/eudi-wallet-rulebooks-and-schemas.git"
    organization: "ewc-consortium"
    layout: "rulebook-catalog"
    options:
      attestation_los: "iso_18045_high"   # TS11 attestation level of security for the whole catalogue
      binding_type: "key"                 # TS11 binding type for the whole catalogue
```

The entry belongs in `sources.yaml` of [registry.siros.org](https://github.com/sirosfoundation/registry.siros.org). The registry rebuilds every six hours.

## What is published

| Input | Output |
|---|---|
| `data-schemas/sd-jwt/dsNNN-<slug>-sd-jwt.json` | `/<org>/<slug>.vctm.json` (SD-JWT VC type metadata), a catalogue entry and its claims |
| `data-schemas/mdoc/dsNNN-<slug>-mdoc.json` | `/<org>/<slug>.mdoc.json` |
| `rulebooks/rb-<slug>/README.md` | `/<org>/<slug>/rulebook.html` and the TS11 `rulebookURI` |
| `assets/<slug>/card.svg`, `card-dark.svg`, `logo.svg` | `svg_templates` and logo in the display metadata |

The slug is the file name without `dsNNN-` and `-sd-jwt.json`; its case is kept. The TS11 API at `/api/v1/schemas.json` lists the attestation types that have a rulebook. The other types are published as type metadata and in the catalogue.

## Adding an attestation type

1. Write `data-schemas/sd-jwt/dsNNN-<slug>-sd-jwt.json` with `properties.vct.const`, `title`, `description`, `version`, claim `description`s and `required`.
2. Write `rulebooks/rb-<slug>/README.md` with attribute tables that match the schema.
3. Open a pull request. The `Validate schemas and registry build` check runs the schema checks and a registry-cli build.

## What registry-cli reads from a schema

- `properties.vct` (`const`, else the first `examples` or `enum` value) is the credential type. A schema without it is skipped.
- `title` is the display name, `description` the description, and each top-level property is a claim. One level of nested `properties` becomes child claims; deeper levels stay in the schema for validation.
- `required` marks claims as mandatory. A claim description that contains "MUST be selectively disclosable" marks the claim as selectively disclosable.
- `iss`, `sub`, `iat`, `exp`, `nbf`, `jti`, `cnf`, `status` and `vct` are not shown as claims.

## Checking a build locally

```sh
go install github.com/sirosfoundation/registry-cli/cmd/registry-cli@v0.18.0
cp -r . /tmp/ewc-src
sed 's#__REPO__#/tmp/ewc-src#' .github/registry-ci-sources.yaml > /tmp/sources.yaml
registry-cli build --sources /tmp/sources.yaml --output /tmp/ewc-out
```

registry-cli writes generated files next to a local source, so build from a copy.

## Card artwork

`assets/<slug>/card.svg` is an optional SVG template for the credential card; `card-dark.svg` and `logo.svg` are optional too. The registry publishes the attestation type without them.

- `viewBox="0 0 400 252"`, with a title and up to five claims as `{{claim}}` placeholders; nested claims as `{{parent.child}}`.
- Each placeholder names a claim of the data schema. A scalar claim renders best.
- The file is self-contained: no `<script>`, `<style>`, `<image>`, `data:` URI or external reference. The text colour has a contrast of at least 4.5:1 with its background.
- The colours follow the domain: identity navy `#0B2A5B`, finance green `#0B5D3B`, company teal `#0E5A63`, mobility purple `#5B2A86`.
