---
name: audit-quality-gates
description: |
  Use this skill when the user wants to audit how a project
  configures its style checkers and static analyzers.
---

## Scope

Operate on project rooted at current working directory.
Refuse when no build manifest is present.

## Restraint

Do not edit, create, delete, or rename any file.
Do not modify build or install dependencies.
Do not wire anything new into task graph.

## Discovery

Identify language ecosystem and build system from manifest first.
Enumerate every style checker and static analyzer already configured.
Record each tool's enabled rule set, threshold settings, and failure mode.

## Comparison

Compare each tool's current config against strictest practical preset.
List every disabled rule.
Check build's failure threshold for each tool.
Flag any tool whose violations build tolerates.

## Suppressions

Detect every suppression in source tree.
Count them per tool.
Surface count as weakness.

## Demands

Read every Markdown file under sibling `demands/` directory.
Treat each demand file as authoritative.
Produce one report entry per demand file.
Do not invent new demands or skip existing ones.
Do not merge two demands into single finding.
Mark every demand as covered, partially covered, or missing.

## Report

Produce single Markdown report.
Deliver it to user.
Structure report with three sections.
Include per-tool section and per-demand coverage section.
End with prioritised list of suggested improvements.
Name exact configuration file, key, and target value.
Do this for every suggested improvement.

## Limits

Do not run build, invoke any linter, or execute any test.
Do not call any tool that mutates caches or lockfiles.
Do not commit, push, or open pull request.
Do not run any other skill from this bundle.
Stop after delivering report.
