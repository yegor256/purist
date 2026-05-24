---
name: audit-quality-gates
description: |
  Use this skill to audit how the project configures
  its style checkers and static analyzers.
---

Operate on the project rooted at the current working directory.
Refuse when no build manifest is present.
Do not edit, create, delete, or rename any file.
Do not modify the build, install dependencies, or wire anything new into the task graph.
Identify the language ecosystem and the build system from the manifest first.
Enumerate every style checker and static analyzer already configured.
Record each tool's enabled rule set, threshold settings, and failure mode.
Compare each tool's current configuration against its strictest practical preset, and list every rule the project has not enabled.
Check the build's failure threshold for each tool, and flag any tool whose violations the build tolerates.
Detect every suppression in the source tree, count them per tool, and surface the count as a weakness.
Read every Markdown file under the `demands/` directory next to this `SKILL.md`.
Treat each demand file as authoritative.
Produce one report entry per demand file.
Do not invent new demands, skip existing ones, or merge two into a single finding.
Mark every demand as covered, partially covered, or missing.
Produce a single Markdown report and deliver it to the user.
Structure the report with a per-tool section, a per-demand coverage section, and a prioritised list of suggested improvements.
Name the exact configuration file, key, and target value for each suggested improvement.
Do not run the build, invoke any linter, execute any test, or call any tool that mutates caches or lockfiles.
Do not commit, push, open a pull request, or run any other skill from this bundle.
Stop after the report has been delivered.
