---
name: audit-quality-gates
description: |
  Use this skill to audit how the project configures
  its style checkers and static analyzers.
---

Operate on the project rooted at the current working directory.
Refuse when no build manifest is present.
Do not edit, create, delete, or rename any file.
Do not modify the build or install dependencies.
Do not wire anything new into the task graph.
Identify the language ecosystem and the build system from the manifest first.
Enumerate every style checker and static analyzer already configured.
Record each tool's enabled rule set, threshold settings, and failure mode.
Compare each tool's current config against the strictest practical preset.
List every rule the project has not enabled.
Check the build's failure threshold for each tool.
Flag any tool whose violations the build tolerates.
Detect every suppression in the source tree.
Count them per tool and surface the count as a weakness.
Read every Markdown file under the sibling `demands/` directory.
Treat each demand file as authoritative.
Produce one report entry per demand file.
Do not invent new demands or skip existing ones.
Do not merge two demands into a single finding.
Mark every demand as covered, partially covered, or missing.
Produce a single Markdown report and deliver it to the user.
Structure the report with three sections.
Include a per-tool section and a per-demand coverage section.
End with a prioritised list of suggested improvements.
Name the exact configuration file, key, and target value.
Do this for every suggested improvement.
Do not run the build, invoke any linter, or execute any test.
Do not call any tool that mutates caches or lockfiles.
Do not commit, push, or open a pull request.
Do not run any other skill from this bundle.
Stop after the report has been delivered.
