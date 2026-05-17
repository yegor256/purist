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

The skill must build and maintain an explicit
  allowlist of entry-point classes for the project
  under audit — never silently skip a class on the
  hunch that it might be reflective.

## Native checks per ecosystem

For Java, write an ArchUnit rule that walks the
  type graph, drops every test class and every
  allowlisted entry point, and asserts that every
  remaining class has at least one inbound reference
  from another non-test class.

For TypeScript, use `ts-prune` or
  `eslint-plugin-import`'s `no-unused-modules` rule
  with the test directories excluded from the
  consumer set.

For Python, use `vulture` configured against the
  production source tree only, with the entry-point
  allowlist passed through `--ignore-names`.

For Ruby, no off-the-shelf cop covers this rule;
  ship the custom checker described below.

For Go, the `unused` analyzer inside `staticcheck`
  catches package-level orphans; combine it with a
  custom check for exported types that have no
  importers.

For Rust, the `dead_code` Clippy lint catches most
  orphans by default; raise it from warning to error
  in the strengthened preset.

## Custom checker fallback

When no native tool fits, ship a checker that parses
  the production source tree, builds a directed
  reference graph from class to class (or module to
  module), removes every node that matches the
  entry-point allowlist, and fails the build on any
  remaining node with in-degree zero.

The checker must distinguish references from
  production code from references from test code,
  and ignore the latter when computing in-degree.

The checker must not flag a class referenced only
  by reflection or by configuration files when the
  user has added that class to the entry-point
  allowlist — flag it otherwise.
