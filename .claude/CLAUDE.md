# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Go package providing English word inflection utilities -- pluralization, singularization,
case conversion (CamelCase, underscore, dasherize, titleize), humanization, ordinalization,
and URL parameterization. It is modeled after Ruby on Rails' ActiveSupport::Inflector and
ships with a comprehensive default ruleset for English. Used throughout the
[go-swagger](https://github.com/go-swagger/go-swagger) ecosystem to transform identifier
names between API specifications and generated Go code.

See [docs/MAINTAINERS.md](../docs/MAINTAINERS.md) for CI/CD, release process, and repo structure details.

### Package layout

| File | Contents |
|------|----------|
| `inflect.go` | All types, rules, and inflection functions: `Ruleset`, `Rule`, default ruleset, and helper utilities |
| `inflect_test.go` | Tests for all inflection functions |

### Key API

- `Ruleset` — configurable collection of pluralization, singularization, humanization, acronym, and uncountable rules
- `NewDefaultRuleset()` — creates a ruleset pre-loaded with common English inflection rules
- `Pluralize(word)` / `Singularize(word)` — convert between plural and singular forms
- `Camelize(word)` — `"dino_party"` -> `"DinoParty"`
- `CamelizeDownFirst(word)` — `"dino_party"` -> `"dinoParty"`
- `Underscore(word)` — `"BigBen"` -> `"big_ben"`
- `Dasherize(word)` — `"SomeText"` -> `"some-text"`
- `Titleize(word)` — `"hello there"` -> `"Hello There"`
- `Humanize(word)` — strips `_id` suffix, applies human-friendly replacements, capitalizes first letter
- `Tableize(word)` — Rails-style table name: `"SuperPerson"` -> `"super_people"`
- `Typeify(word)` — `"something_like_this"` -> `"SomethingLikeThis"` (strips table prefix, singularizes, camelizes)
- `ForeignKey(word)` / `ForeignKeyCondensed(word)` — `"Person"` -> `"person_id"` / `"personid"`
- `Ordinalize(str)` — `"1031"` -> `"1031st"`
- `Parameterize(word)` / `ParameterizeJoin(word, sep)` — URL-safe dasherized strings
- `Asciify(word)` — transliterates Latin characters (`e` -> `e`)
- `AddPlural`, `AddSingular`, `AddIrregular`, `AddUncountable`, `AddHuman`, `AddAcronym` — extend the default ruleset

### Dependencies

None -- this package uses only the Go standard library.

### Notable design decisions

- **Suffix-based matching, not regexps** — rules match by string suffix (or exact match), making
  the rule engine simple and fast. Rules are checked in reverse insertion order (most recently
  added rule wins).
- **Global default ruleset** — a package-level `defaultRuleset` is initialized in `init()` and
  exposed via top-level convenience functions (`Pluralize`, `Underscore`, etc.). Users who need
  isolated rule sets can create their own via `NewRuleset()` or `NewDefaultRuleset()`.
- **Acronym-safe case conversion** — `AddAcronym("HTML")` prevents `Underscore` from splitting
  it into `h_t_m_l`; the acronym is treated as a single unit during case-change splitting.
