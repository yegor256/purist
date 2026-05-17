# A line is indented by one tab, not more

Inside every method body, no line is indented more
  than one tab past the indentation of the method
  signature itself — the body may sit at one nested
  level, never deeper.

The rule bans every form of nesting inside a method:
  a loop inside a loop, an `if` inside a loop, an
  `if` inside an `if`, a `try` inside an `if`, a
  lambda inside a conditional whose body itself
  branches, and so on.

The rule measures depth in indentation steps, not in
  AST nesting nodes — one indentation step past the
  method signature is allowed, two is a violation;
  the project's tab width or space count is the unit
  of measurement.

## Native check coverage per ecosystem

For Java, Checkstyle's `NestedIfDepth`,
  `NestedForDepth`, and `NestedTryDepth` each set to
  `max=0`, together with a PMD `CyclomaticComplexity`
  threshold low enough to flag any nested branch,
  covers the demand; the audit reports the project as
  covered only when all three Checkstyle modules and
  a tight PMD threshold are in place.

For Kotlin, Detekt's `NestedBlockDepth` rule with
  `threshold=1` covers the demand; the audit reports
  the project as covered only when the rule is on at
  that threshold.

For JavaScript and TypeScript, ESLint's `max-depth`
  rule set to `["error", 1]` together with
  `max-nested-callbacks` set to `["error", 1]` covers
  the demand; the audit reports the project as
  covered only when both rules are on at that
  setting.

For Python, Pylint's `max-nested-blocks=1` covers
  the demand; the audit reports the project as
  covered only when the option is set to `1`.

For Ruby, RuboCop's `Metrics/BlockNesting` with
  `Max: 1` covers the demand; the audit reports the
  project as covered only when the cop is on at that
  threshold.

For Go, no native linter ships a nesting-depth knob;
  the audit reports the demand as missing whenever no
  custom checker is wired into the build.

For Rust, no Clippy lint enforces a nesting limit;
  the audit reports the demand as missing whenever no
  custom checker is wired into the build.

## Custom checker recommendation

When no native check fits, the audit recommends an
  AST-based checker that walks every method,
  function, or block, computes the maximum nesting
  depth of statements inside its body, and fails the
  build on any method whose deepest statement sits
  more than one level past the method's own
  indentation.

The recommended checker must count each `if`, `for`,
  `while`, `do`, `try`, `catch`, `switch`, `with`,
  and block expression as one nesting level, and
  must not count guard clauses (`if x: return`,
  `if (x) return;`) that exit the method without a
  nested body of their own.

The recommended checker must skip files under the
  project's test source roots — this demand applies
  to production code only.
