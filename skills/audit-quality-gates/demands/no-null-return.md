# No method returns `null`

A method must never return a null reference of any
  kind — return an empty collection, an empty string,
  an `Optional`, a `Maybe`, a typed `None` value, or
  throw an exception — but never the language's null
  literal.

The rule covers the language's native null literal
  (`null` for Java/JavaScript/TypeScript/Kotlin, `None`
  for Python, `nil` for Ruby/Go, `Option::None` for
  Rust) and every alias the project introduces for it.

## Native check coverage per ecosystem

For Java, PMD's `ReturnEmptyCollectionRatherThanNull`
  and SpotBugs' `NP_BOOLEAN_RETURN_NULL` together
  cover collections and boxed booleans, but not the
  general case; the audit reports the project as
  partially covered when both are enabled, and as
  missing when either is off.

For Kotlin, the compiler's `-Xexplicit-api=strict`
  flag combined with a Detekt `ReturnNullable` rule
  forbids nullable return types in production code;
  the audit reports the project as covered only when
  both are configured.

For JavaScript and TypeScript, ESLint's `no-null`
  catches the literal and, in TypeScript projects,
  `strictNullChecks` plus
  `@typescript-eslint/no-explicit-any` close the path
  by which a null return can hide behind `any`; the
  audit reports the project as covered only when all
  three are on.

For Python, no off-the-shelf linter rejects every
  `return None`; the audit reports the demand as
  missing whenever no custom checker is wired into
  the build.

For Ruby, no off-the-shelf RuboCop cop rejects every
  `return nil`; the audit reports the demand as
  missing whenever no custom checker is wired into
  the build.

For Go, neither `go vet` nor `staticcheck` forbids
  bare `return nil` from non-error positions; the
  audit reports the demand as missing whenever no
  custom checker is wired into the build.

For Rust, no Clippy lint bans `Option::None` literal
  returns; the audit reports the demand as missing
  whenever no custom Clippy lint or AST checker is
  wired into the build.

## Custom checker recommendation

When the language has no native check that covers
  the general case, the audit recommends a small
  AST-based checker that parses every production
  source file, walks every method body, and fails the
  build on any return statement whose value is the
  language's null literal.

The recommended checker must not flag null literals
  used outside of `return` statements — assignments
  to local variables, default parameters, and field
  initializers are out of scope of this demand.

The recommended checker must skip every file under
  the project's test source roots — this demand
  applies to production code only.
