# License & Source Policy

## Purpose

The Ginkgo Library catalogs and analyzes third-party technologies. It must not accidentally convert research into unauthorized redistribution.

## Default behavior

For third-party projects, store:

- canonical repository/site URL;
- license identifier when known;
- version/commit when relevant;
- Ginkgo analysis and notes;
- architecture diagrams created by Ginkgo;
- short necessary excerpts only when appropriate;
- provenance references.

Do not copy entire upstream repositories into this library by default.

## Open source

Before promoting or reusing code, verify the actual current license in the upstream source.

Record:

```yaml
license: Apache-2.0
```

or:

```yaml
license: unknown
```

Never infer a permissive license from a public GitHub repository. Public code is not automatically open source.

## Forking/vendor copies

Fork or preserve third-party code only when there is a documented reason:

- Ginkgo patches are required;
- upstream stability/preservation matters;
- self-hosting needs a controlled fork;
- supply-chain review requires pinning;
- upstream is abandoned and license permits maintenance.

When doing so, preserve upstream license/attribution requirements.

## Commercial platforms

Document features, APIs, pricing models and use cases, but do not copy proprietary documentation wholesale.

Record terms/data-policy risks before production adoption.

## Clippings

Raw clippings are research provenance, not a public redistribution archive.

Avoid uploading:

- paid course content in full;
- books or large copyrighted works;
- proprietary client documents;
- leaked/internal vendor material;
- content obtained through access circumvention.

Prefer summaries, metadata, canonical links and your own notes.

## Unknown license

An item with unknown or unclear license may be researched and referenced, but code should not be reused until licensing is resolved.
