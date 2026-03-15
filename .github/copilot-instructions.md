# Copilot Instructions

## Project Overview

Go package providing English word inflection utilities -- pluralization, singularization,
case conversion (CamelCase, underscore, dasherize, titleize), humanization, ordinalization,
and URL parameterization. Modeled after Ruby on Rails' ActiveSupport::Inflector. Used in the
[go-swagger](https://github.com/go-swagger/go-swagger) ecosystem to transform identifier
names between API specifications and generated Go code.

### Package layout

| File | Contents |
|------|----------|
| `inflect.go` | All types, rules, and inflection functions |
| `inflect_test.go` | Tests for all inflection functions |

### Key API

- `Ruleset` — configurable rule collection (plural, singular, human, acronym, uncountable)
- `NewDefaultRuleset()` — ruleset pre-loaded with common English rules
- `Pluralize` / `Singularize` — plural/singular conversion
- `Camelize` / `CamelizeDownFirst` / `Underscore` / `Dasherize` / `Titleize` — case conversion
- `Humanize` / `Tableize` / `Typeify` / `ForeignKey` / `Ordinalize` / `Parameterize` — specialized transforms
- `Asciify` — Latin character transliteration
- `AddPlural`, `AddSingular`, `AddIrregular`, `AddUncountable`, `AddHuman`, `AddAcronym` — extend rules

### Dependencies

None -- standard library only.

### Notable design decisions

- Suffix-based matching (not regexps) for simplicity and speed.
- Global default ruleset via `init()` with top-level convenience functions.
- Acronym-safe case conversion via `AddAcronym()`.

## Conventions

Coding conventions are found beneath `.github/copilot`

### Summary

- All `.go` files must have SPDX license headers (Apache-2.0).
- Commits require DCO sign-off (`git commit -s`).
- Linting: `golangci-lint run` — config in `.golangci.yml` (posture: `default: all` with explicit disables).
- Every `//nolint` directive **must** have an inline comment explaining why.
- Tests: `go test ./...`. CI runs on `{ubuntu, macos, windows} x {stable, oldstable}` with `-race`.
- Test framework: `github.com/go-openapi/testify/v2` (not `stretchr/testify`; `testifylint` does not work).

See `.github/copilot/` (symlinked to `.claude/rules/`) for detailed rules on Go conventions, linting, testing, and contributions.
