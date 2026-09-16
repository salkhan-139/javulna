# Auto-merge criteria

These criteria are additive to the organization-level auto-merge settings. A PR
must satisfy both. When a PR fails these criteria, leave the merge to a human
reviewer.

## Only arm auto-merge when the change is low-impact

Arm auto-merge only when every changed file in the PR falls into one of these
categories:

- Markdown documentation: `readme.md`, and any other `*.md` outside `.gitar/`.
- Comment-only, whitespace-only, or formatting-only edits to Java source.
- Non-functional string changes to user-facing copy in JSP or HTML views, where
  no logic, attribute, or expression is altered.
- Test-only additions under `src/test/` that add coverage without changing
  production code.
- `.gitignore`.

Keep the diff small. Do not arm auto-merge when the PR changes more than about
ten files, or when a single file accumulates more than roughly 150 changed
lines, even if every path is otherwise eligible.

## Never arm auto-merge on high-impact changes

Never arm auto-merge on a PR that touches any of the following, regardless of
how small the diff is. Route these to a human.

Infrastructure and deployment:

- Anything under `terraform/`, any `*.tf`, `*.tfvars`, or generated
  `artifacts/tfplan.json`. These are governed by the HLD conformance rule.
- CI or pipeline definitions, including `.github/`, and any build or deploy
  script.

Build and dependencies:

- `pom.xml`, any Maven wrapper or build plugin configuration, or any change that
  adds, removes, or upgrades a dependency or changes a version.

Security-sensitive application code:

- Anything under `src/main/java/com/kalavit/javulna/springconfig/` or
  `src/main/java/com/kalavit/javulna/filter/`, which carry the application's
  security and request-filter wiring.
- Anything under `src/main/java/com/kalavit/javulna/controllers/`, including the
  `rest/` subpackage, since these define the externally reachable request
  surface.
- Any code handling authentication, authorization, session or cookie handling,
  password or credential logic, cryptography, serialization or deserialization,
  file upload or path resolution, command execution, redirects, or SQL and LDAP
  query construction.
- Anything under `src/main/java/com/kalavit/javulna/services/autodao/` or any
  change to persistence or query-building logic.

Configuration and policy:

- `doc/mypolicy.policy` and `doc/ldap.ldif`. Despite living under `doc/`, these
  are a Java security policy and a directory configuration, not documentation.
- Anything under `src/main/resources/`, including `src/main/resources/xml/`.
- Any properties, YAML, XML, or environment configuration file.

UI, styling, and fonts:

- Anything under `src/main/webapp/styles/`, any CSS or SCSS file, any
  `@font-face` declaration, font import, CDN font URL, bundled font asset, or
  design-token or theme file. These are governed by the font licensing rule.

Governance and secrets:

- Anything under `.gitar/`, `.claude/`, `AGENTS.md`, or `CLAUDE.md`. A PR that
  changes how review or merge is governed must be reviewed by a human.
- Any file that introduces a credential, token, key, certificate, or other
  secret material.
- Any database migration or schema change.

## Additional holds

Also leave the merge to a human when:

- The PR description is empty, or is too thin to establish intent.
- The PR is marked draft, or its title or description signals uncertainty, such
  as "WIP", "experimental", "temporary", "hotfix", "revert", or "do not merge".
- An open review finding from any Gitar rule has not been resolved, including
  findings labelled `hld-conformance-review` or `font-license-review`.
- The PR mixes an eligible low-impact path with any high-impact path. Mixed PRs
  are never armed; ask the author to split them.
