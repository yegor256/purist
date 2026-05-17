---
name: enforce-quality
description: |
  Use this skill to harden the style checkers and
  static analyzers already wired into the project —
  discover every configuration file in place, raise
  each tool to its strictest practical setting, and
  then enforce every demand declared under
  `demands/` in the skill directory through the
  project's native checks or, when no native check
  fits, through a custom checker. One project per
  run, one hardening pass per run — then stop.
---

Operate on the project rooted at the current working
  directory; refuse to run when the directory has no
  build manifest (`pom.xml`, `build.gradle`,
  `package.json`, `pyproject.toml`, `Gemfile`,
  `Cargo.toml`, `go.mod`, or equivalent) and no source
  tree to anchor the audit against.

Refuse to run when the working tree has uncommitted
  changes — staged, unstaged, or untracked — because
  this skill rewrites configuration files and a clean
  baseline lets the user review the resulting diff in
  isolation.

Identify the language ecosystem and the build system
  from the manifest before touching any rule file —
  Maven, Gradle, npm, pnpm, Yarn, Poetry, pip, Bundler,
  Cargo, or `go build` — because every later step
  depends on knowing which tool runs which check.

Enumerate every style checker and static analyzer
  already configured in the project — Checkstyle, PMD,
  SpotBugs, ErrorProne, Qulice for the JVM; ESLint,
  Prettier, Biome, `tsc` for JavaScript and TypeScript;
  Pylint, Flake8, Ruff, Mypy, Bandit for Python;
  RuboCop for Ruby; `golangci-lint`, `go vet`,
  `staticcheck` for Go; Clippy for Rust — and record
  the path to each configuration file.

Refuse to add a brand-new style checker or static
  analyzer that the project does not already use —
  the skill hardens what is configured, never bolts on
  a foreign tool the maintainer never approved.

For each discovered tool, replace the current
  configuration with the strictest practical preset —
  enable every published rule, raise every threshold
  to its hardest setting, and disable a rule only when
  it directly contradicts another enabled rule.

Examples of the strictest preset per tool: Checkstyle
  bound to `sun_checks.xml` plus every additional
  module the version ships; PMD with every built-in
  category enabled; ESLint with `eslint:all` plus
  `@typescript-eslint/all`; Pylint with no disabled
  messages and `--enable=all`; Ruff with `select = ["ALL"]`;
  RuboCop with `AllCops/NewCops: enable` and no
  blanket `Metrics` opt-outs; `golangci-lint` with
  every linter under `linters: enable-all: true`.

Document each disabled rule with a single-line
  comment in the configuration file naming the
  contradicting rule, because a silent exclusion looks
  like a casual opt-out and a noted one looks like a
  deliberate trade-off.

Raise the build's failure threshold to zero warnings:
  `-Werror` for the JVM compiler, `--max-warnings=0`
  for ESLint, `--fail-under=10` for Pylint, and the
  equivalent flag for every other tool — a warning the
  build tolerates is a warning nobody fixes.

Read every Markdown file under the `demands/`
  directory next to this `SKILL.md` before touching a
  single source file — each file declares one rule
  the strengthened presets do not cover, names the
  native check to prefer per language, and names the
  custom-checker fallback to ship when no native
  check fits.

Treat the file name of every demand as authoritative
  — do not invent new demands, do not skip existing
  ones, and do not merge two demands into a single
  checker — one file means one rule means one check.

For every demand, enforce the rule it names through
  the native check the demand lists for the project's
  discovered toolchain, and ship a custom checker
  only when the demand explicitly states that no
  native check covers the language at hand.

Place every custom checker wherever the project
  already keeps its build extensions, plugin sources,
  or scripts — follow the layout the maintainer
  established, do not invent a new top-level directory
  — and write each one in the language the project
  already builds with so the maintainer does not
  inherit a new toolchain.

Wire each custom checker into the build's main task
  graph — `mvn verify`, `gradle check`, `npm run
  lint`, `pytest --strict`, `cargo clippy`, `go vet`,
  whichever phase already fails the build on lint
  errors — because a quality rule that does not break
  the build is a comment, not a check.

Refuse to silence an existing violation by adding a
  suppression annotation, a baseline file, an
  `eslint-disable` marker, a `noqa`, a `# type: ignore`,
  or any equivalent escape hatch — fix the source or
  report the count back to the user, because a
  baselined violation is permanent debt the next
  reader inherits.

Run the project's full build once on the strengthened
  configuration before reporting back — `mvn verify`,
  `gradle build`, `npm test`, `pytest`, `cargo test`,
  `go test ./...`, whichever command the manifest
  declares — and surface the first failing rule and
  its file location to the user when the build does
  not pass.

Do not commit the changes, do not push, do not open a
  pull request, and do not run any other skill from
  this bundle — leave the diff on the working tree for
  the user to review, amend, and ship through their
  own workflow.

Stop after the strengthened build has been run and
  its result reported; do not propose a follow-up
  tool, do not refactor unrelated source code, and do
  not re-run on a sibling project — invoke this skill
  again from the top for the next project.
