---
name: audit-quality-gates
description: |
  Use this skill to audit how the project configures
  its style checkers and static analyzers — locate
  every configuration file in place, compare each
  tool's current settings against its strictest
  practical preset, compare the project's checks
  against every rule declared under `demands/` in the
  skill directory, and report the gaps, weaknesses,
  and concrete improvements back to the user. One
  project per run, one audit per run — then stop.
---

Operate on the project rooted at the current working
  directory; refuse to run when the directory has no
  build manifest (`pom.xml`, `build.gradle`,
  `package.json`, `pyproject.toml`, `Gemfile`,
  `Cargo.toml`, `go.mod`, or equivalent) and no source
  tree to anchor the audit against.

Read configuration files only; do not edit, create,
  delete, or rename any file in the working tree, do
  not modify the build, do not install dependencies,
  and do not wire anything new into the task graph —
  this skill produces a written report, nothing else.

Identify the language ecosystem and the build system
  from the manifest before reading any rule file —
  Maven, Gradle, npm, pnpm, Yarn, Poetry, pip, Bundler,
  Cargo, or `go build` — because every later judgement
  depends on knowing which tool runs which check.

Enumerate every style checker and static analyzer
  already configured in the project — Checkstyle, PMD,
  SpotBugs, ErrorProne, Qulice for the JVM; ESLint,
  Prettier, Biome, `tsc` for JavaScript and TypeScript;
  Pylint, Flake8, Ruff, Mypy, Bandit for Python;
  RuboCop for Ruby; `golangci-lint`, `go vet`,
  `staticcheck` for Go; Clippy for Rust — and record
  the path to each configuration file.

Record each tool's currently enabled rule set, its
  threshold settings, and its failure mode — warn,
  error, or build-break — because the audit compares
  the live state against the strictest practical
  preset and cannot judge a tool whose state was not
  captured.

Compare each tool's current configuration against its
  strictest practical preset — Checkstyle bound to
  `sun_checks.xml` plus every additional module the
  version ships; PMD with every built-in category
  enabled; ESLint with `eslint:all` plus
  `@typescript-eslint/all`; Pylint with no disabled
  messages and `--enable=all`; Ruff with
  `select = ["ALL"]`; RuboCop with `AllCops/NewCops:
  enable` and no blanket `Metrics` opt-outs;
  `golangci-lint` with every linter under
  `enable-all: true`; Clippy with `-W clippy::pedantic
  -W clippy::nursery` — and list every rule the project
  has not enabled.

Check the build's failure threshold for each tool —
  `-Werror` for the JVM compiler, `--max-warnings=0`
  for ESLint, `--fail-under=10` for Pylint, the
  equivalent flag elsewhere — and flag any tool that
  treats violations as warnings the build tolerates,
  because a warning the build tolerates is a warning
  nobody fixes.

Detect every suppression already living in the source
  tree — `@SuppressWarnings`, `eslint-disable`, `noqa`,
  `# type: ignore`, RuboCop `:disable`, Checkstyle
  `SuppressWarnings`, baseline files, ignore lists —
  count them per tool and surface the count as a
  weakness, because each suppression is a check the
  next reader inherits as permanent debt.

Read every Markdown file under the `demands/`
  directory next to this `SKILL.md` — each file
  declares one rule the project must enforce, names
  the native check to prefer per language, and names
  the custom-checker fallback to recommend when no
  native check fits.

Treat the file name of every demand as authoritative
  — do not invent new demands, do not skip existing
  ones, and do not merge two demands into a single
  finding — one file means one rule means one entry
  in the report.

For every demand, check whether the project enforces
  it through the native check the demand lists or
  through an equivalent custom checker, and mark the
  demand as covered, partially covered, or missing in
  the report.

Produce a single Markdown report delivered to the
  user, structured as: a per-tool section naming the
  current preset, the gap against the strictest
  preset, the failure-threshold weakness, and the
  suppression count; followed by a per-demand section
  naming each demand and its coverage state; followed
  by a prioritised list of suggested improvements,
  each naming the exact configuration file, key, and
  target value to change.

Do not run the project's build, do not invoke any
  linter, do not execute any test, and do not call any
  tool that mutates caches or lockfiles — the audit
  reads files, it does not run builds.

Do not commit, do not push, do not open a pull
  request, and do not run any other skill from this
  bundle — leave the working tree exactly as it was
  found.

Stop after the report has been delivered; do not
  propose a follow-up tool, do not refactor unrelated
  source code, and do not re-run on a sibling project
  — invoke this skill again from the top for the next
  project.
