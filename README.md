# A Claude Code Plugin for Auditing Code Quality Gates

[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/yegor256/purist/blob/master/LICENSES/MIT.txt)

A single Claude Code skill that audits the style
  checkers and static analyzers already wired into a
  project — it does not bolt on a new toolchain and it
  does not edit a single configuration file; it reads
  every checker the maintainer already approved,
  compares the live settings against the strictest
  practical preset and against a list of extra rules
  that the default presets miss, and reports the gaps
  back to the user.

The bundle ships exactly one skill:

* [`audit-quality-gates`](skills/audit-quality-gates/SKILL.md)
  — discover every configured checker, compare each
    one against its strictest preset and against the
    extra rules the skill defines, and deliver a
    written report of gaps, weaknesses, and suggested
    improvements.

Suppose you work with [Claude Code].
You do not need to clone this repository — install the bundle as a
  plugin straight from GitHub.
Inside a Claude Code session, run:

```text
/plugin marketplace add yegor256/plugins
/plugin install purist@yegor256
```

The first command registers the [yegor256/plugins] marketplace,
  which lists every plugin maintained under the `yegor256` account;
  the second installs the `purist` plugin from it,
  which exposes the `audit-quality-gates` skill to your sessions
  automatically.

To update later, run `/plugin marketplace update yegor256`;
  to remove, run `/plugin uninstall purist@yegor256`.

[yegor256/plugins]: https://github.com/yegor256/plugins

[Claude Code]: https://code.claude.com/docs/en/skills
