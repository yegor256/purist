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

## Native checks per ecosystem

For Java, enable Checkstyle's `NestedIfDepth`,
  `NestedForDepth`, and `NestedTryDepth` each set to
  `max=0`, and PMD's `CyclomaticComplexity`
  threshold low enough to flag any nested branch.

For Kotlin, enable Detekt's `NestedBlockDepth` rule
  with `threshold=1`.

For JavaScript and TypeScript, enable ESLint's
  `max-depth` rule set to `["error", 1]` and
  `max-nested-callbacks` set to `["error", 1]`.

For Python, set Pylint's `max-nested-blocks=1` in
  the strengthened configuration.

For Ruby, set RuboCop's `Metrics/BlockNesting`
  `Max: 1`.

For Go, no native linter ships a nesting-depth
  knob; ship the custom checker described below.

For Rust, no Clippy lint enforces a nesting limit;
  ship the custom checker described below.

## Custom checker fallback

When no native check fits, ship an AST-based
  checker that walks every method, function, or
  block, computes the maximum nesting depth of
  statements inside its body, and fails the build on
  any method whose deepest statement sits more than
  one level past the method's own indentation.

The checker must count each `if`, `for`, `while`,
  `do`, `try`, `catch`, `switch`, `with`, and block
  expression as one nesting level, and must not
  count guard clauses (`if x: return`,
  `if (x) return;`) that exit the method without a
  nested body of their own.

The checker must skip files under the project's
  test source roots — this demand applies to
  production code only.
