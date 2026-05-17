# A Claude Code Plugin for Enforcing Code Quality

[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/yegor256/purist/blob/master/LICENSES/MIT.txt)

A single Claude Code skill that hardens the style
  checkers and static analyzers already wired into a
  project — it does not bolt on a new toolchain; it
  cranks the configuration the maintainer already
  approved up to its strictest practical setting and
  then enforces a list of extra rules that the default
  presets miss.

The bundle ships exactly one skill:

* [`enforce-quality`](skills/enforce-quality/SKILL.md)
  — discover every configured checker, raise each one
    to its strictest preset, and add the custom rules
    the skill defines on top.

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
  which exposes the `enforce-quality` skill to your sessions
  automatically.

To update later, run `/plugin marketplace update yegor256`;
  to remove, run `/plugin uninstall purist@yegor256`.

[yegor256/plugins]: https://github.com/yegor256/plugins

[Claude Code]: https://code.claude.com/docs/en/skills
