# Paired Brackets

Every opening bracket — `(`, `{`, `[`, `<` — either
  closes on the same line where it opens, or its
  matching closing bracket sits alone on a line at
  the same indentation column as the first
  non-whitespace character of the line that opened
  it.

The rule, defined by Yegor Bugayenko in his
  ["Paired Brackets Notation"](https://www.yegor256.com/2014/10/23/paired-brackets-notation.html)
  blog post, bans the dangling-closer layout (`}`)
  and the K&R-style mid-line opener followed by a
  same-indent closer; the closer must align with the
  opening line, not with the opener itself.

The rule applies to every bracket pair: function
  call parens, code blocks, generics, array
  literals, dictionary literals, and angle-bracket
  type arguments.

## Examples

A single-line pair is fine:

```
return doStuff(x, y, z);
```

A multi-line pair must align the closer with the
  start of the opening line, not with the opener:

```
return doStuff(
    x,
    y,
    z
);
```

A multi-line pair laid out the K&R way violates the
  rule — the closing brace aligns with `if` instead
  of with the opening line's first column:

```
if (x) {
    doStuff();
}
```

Under Paired Brackets the same block reads:

```
if (x)
  {
    doStuff();
  }
```

## Native check coverage per ecosystem

No mainstream linter ships a complete Paired
  Brackets check today — Checkstyle's `RightCurly`,
  ESLint's `brace-style`, Prettier's wrapping, and
  RuboCop's `Layout/EndAlignment` each cover a
  fragment of the rule but disagree with it on the
  multi-line case.

The audit treats every ecosystem as missing native
  coverage for this demand until a native rule that
  matches the full specification appears, and reports
  the demand as covered only when a custom checker
  matching the specification below is wired into the
  build.

## Custom checker recommendation

The audit recommends a lexer-based checker that
  scans every production source file token by token,
  pairs every opening bracket with its matching
  closing bracket, and fails the build on any pair
  whose closer is neither on the opener's line nor
  on a line whose first non-whitespace column matches
  the column of the opener-line's first non-whitespace
  character.

The recommended checker must handle strings,
  character literals, comments, and language-specific
  bracket-like tokens (Java generics, TypeScript
  generics, Python f-string expressions) without
  miscounting them as brackets.

The recommended checker must skip files under the
  project's test source roots — this demand applies
  to production code only.
