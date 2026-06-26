# @artsy/changelog

Shared parser for Artsy's PR-description changelog convention. A single source of truth used by
both [eigen](https://github.com/artsy/eigen) (Danger + `generate-changelog`) and
[release-lookout](https://github.com/artsy/release-lookout) (release-candidate PR descriptions).

Artsy PRs declare release notes in four `####`-headed sections inside the PR body:

```
#### Cross-platform user-facing changes
#### iOS user-facing changes
#### Android user-facing changes
#### Dev changes
```

This package parses a raw PR body into a structured result. It is intentionally **I/O-free** —
fetching PRs / determining ranges is left to each consumer.

## Usage

```ts
import { parsePRDescription, changelogTemplateSections } from "@artsy/changelog"

const result = parsePRDescription(pr.body)

switch (result.type) {
  case "no_changes": // PR contains `#nochangelog`
    break
  case "error": // no changelog sections found / all empty
    break
  case "changes":
    result.crossPlatformUserFacingChanges // string[]
    result.iOSUserFacingChanges
    result.androidUserFacingChanges
    result.devChanges
    break
}
```

## API

- `parsePRDescription(body: string): ParseResult` — parse a PR body.
- `ParseResult` — `{ type: "no_changes" } | { type: "error" } | ({ type: "changes" } & ParseResultChanges)`.
- `changelogTemplateSections` — the section-key → section-title map.

## Development

```sh
yarn        # install
yarn test   # jest
yarn build  # tsc → dist
```
