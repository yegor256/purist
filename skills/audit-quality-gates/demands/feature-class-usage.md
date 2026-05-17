# Every feature class is used

Every production class in the source tree must be
  referenced by at least one other production class
  — an orphan production class with zero non-test
  callers violates the rule.

A "feature class" is any class, type, module, or
  top-level construct that lives outside the
  project's test source roots and outside any
  generated-source directory the build tool
  produces.

Test sources never count as users — a class
  referenced only by tests is still an orphan in the
  eyes of this demand, because tests verify behavior
  rather than create it.

## Allowed entry points

Declared entry points do not count as orphans even
  when no other class references them, because the
  runtime, the framework, or the build tool reaches
  them through a mechanism the static graph cannot
  see.

The entry-point set includes: `public static void
  main(String[])` methods; framework bootstrap
  classes registered through annotations
  (`@SpringBootApplication`, `@QuarkusMain`, etc.);
  classes named in `META-INF/services/` SPI files;
  classes named in a JAR manifest's `Main-Class`
  attribute; CLI command classes registered through
  a framework router; HTTP controllers or message
  handlers wired by routing or by annotation
  scanning.

The audit reports the demand as fully covered only
  when the project's analyzer configuration carries
  an explicit allowlist of these entry points — a
  silent skip on the hunch that a class might be
  reflective counts as missing coverage, not as
  presence.

## Native check coverage per ecosystem

For Java, an ArchUnit rule that walks the type
  graph, drops every test class and every allowlisted
  entry point, and asserts a non-zero inbound
  reference count for every remaining class covers
  the demand; the audit reports the project as
  covered only when such a rule is wired into the
  build.

For TypeScript, `ts-prune` or
  `eslint-plugin-import`'s `no-unused-modules` rule
  with the test directories excluded from the
  consumer set covers the demand; the audit reports
  the project as covered only when one of the two is
  wired into the build with the test exclusion in
  place.

For Python, `vulture` configured against the
  production source tree only, with the entry-point
  allowlist passed through `--ignore-names`, covers
  the demand; the audit reports the project as
  covered only when `vulture` is wired into the build
  with the production scope and allowlist in place.

For Ruby, no off-the-shelf cop covers this rule; the
  audit reports the demand as missing whenever no
  custom checker is wired into the build.

For Go, the `unused` analyzer inside `staticcheck`
  catches package-level orphans but does not catch
  exported types without importers; the audit reports
  the project as partially covered when only
  `staticcheck` runs, and as fully covered only when
  a custom checker for exported orphans runs
  alongside it.

For Rust, Clippy's `dead_code` lint catches most
  orphans by default; the audit reports the project
  as fully covered only when the lint is raised from
  warning to error.

## Custom checker recommendation

When no native tool fits, the audit recommends a
  checker that parses the production source tree,
  builds a directed reference graph from class to
  class (or module to module), removes every node
  that matches the entry-point allowlist, and fails
  the build on any remaining node with in-degree
  zero.

The recommended checker must distinguish references
  from production code from references from test
  code, and ignore the latter when computing
  in-degree.

The recommended checker must not flag a class
  referenced only by reflection or by configuration
  files when the project's allowlist names that
  class — flag it otherwise.
