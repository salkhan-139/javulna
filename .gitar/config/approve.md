# Auto-approve criteria

These criteria are additive to the organization-level auto-approve settings. A
PR must satisfy both.

Auto-approve is evaluated separately from auto-merge. Withholding approval here
is what forces a human to actually review a high-impact change, rather than
merely clicking merge on a change Gitar already approved.

## Withhold auto-approval on high-impact changes

Do not auto-approve a PR that touches any of the high-impact surfaces listed in
`.gitar/config/merge.md`. In particular, require a human approval when the PR
changes:

- Infrastructure: anything under `terraform/`, any `*.tf` or `*.tfvars`, or a
  generated Terraform plan.
- Build and dependencies: `pom.xml`, or any dependency addition, removal, or
  version change.
- Security-sensitive application code: the `springconfig/`, `filter/`,
  `controllers/`, and `services/autodao/` packages, or any code handling
  authentication, authorization, sessions, credentials, cryptography,
  serialization, file upload or path resolution, command execution, redirects,
  or SQL and LDAP query construction.
- Configuration and policy: anything under `src/main/resources/`, plus
  `doc/mypolicy.policy` and `doc/ldap.ldif`.
- UI, styling, and fonts: anything under `src/main/webapp/styles/`, any CSS or
  SCSS file, font declaration, font asset, or theme or design-token file.
- Governance: anything under `.gitar/`, `.claude/`, `AGENTS.md`, or `CLAUDE.md`.
- Any credential, token, key, or certificate, and any database migration.

## Withhold auto-approval when review is inconclusive

Also require a human approval when:

- Any review finding is open and unresolved, including findings labelled
  `hld-conformance-review` or `font-license-review`.
- The terraform-to-HLD review reported an "HLD ambiguity" rather than a clean
  pass, since a missing requirement cannot be inferred.
- A detected font family is ambiguous rather than clearly on the approved list
  in `.gitar/documents/approved-fonts.md`.
- The PR description does not establish intent, or the PR is draft or signals
  uncertainty in its title or description.
- The change is large enough that a clean review is not a strong signal, as a
  rough guide more than about ten files or 150 changed lines in one file.

## Otherwise approve on a clean review

When the review is clean and the change is confined to the low-impact surface
defined in `.gitar/config/merge.md` — Markdown documentation, comment,
whitespace or formatting-only Java edits, non-functional view copy, test-only
additions under `src/test/`, or `.gitignore` — auto-approve is appropriate.
