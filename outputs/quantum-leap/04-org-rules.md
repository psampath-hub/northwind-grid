# Target Org Rules — Northwind Grid

> Reference role: hard constraints on what the build agent may and may not do in the target org. Read once; never violate without explicit user override.

## Org identity
- **Name:** `Northwind Grid CCF Sandbox`
- **Type:** sandbox
- **Build allowed:** yes
- **Posture:** Brownfield — building into an existing org; respect what's already there and make additive changes only

## Hard rules

- This is a **brownfield** build — the target org already exists with live configuration, data, and likely users. Existing config is load-bearing until proven otherwise.
- **Additive changes only.** Do not rename, repurpose, or delete existing objects, fields, automation, permission sets, or layouts without an explicit user instruction.
- **Inventory before you build.** On first connection, describe the org (objects, fields, automation, permission model in scope) and reconcile against `03-glossary-and-naming.md`. If a name you intend to create already exists, stop and ask whether to extend it or pick a new name.
- **Collision awareness.** Check for existing API names, automation on the same object/trigger context, and managed-package namespaces before creating metadata — avoid clobbering or duplicating existing behavior.
- Do not deploy to production without an explicit `"deploy to production"` instruction from the user.
- Permission sets only — do not modify standard profiles (System Administrator, Standard User) or existing profiles.
- Idempotent: before creating any metadata, check whether it already exists. Update in place only when the spec has changed **and** the change is additive or user-approved.
- Use API names from `03-glossary-and-naming.md`. If a name is not listed, ask before inventing — and check it doesn't already exist in the org.
- **Data residency:** scope mentions a residency consideration. Surface this to the user before creating any object that may store regulated data; confirm the target region.

## Existing customizations
**Assume the org already carries customizations.** Before building, inventory existing objects, fields, automation, and permission sets relevant to this phase, and reconcile them against `03-glossary-and-naming.md`. Reuse and extend what fits; never modify or delete existing config without explicit user approval. If you find a customization that conflicts with the scoped build, stop and surface it.

## Profiles to leave untouched
- System Administrator
- Standard User
- Any other standard profile

## Managed packages
_(The org may already have managed packages installed. Detect them on connection and respect their namespaces — do not assume a clean package state. If a new managed-package install becomes necessary mid-build, surface it to the user before installing.)_

## Operational rules
- **Sandbox first.** Production deploys require an explicit `"deploy to production"` instruction from the user.
- **Idempotent builds.** Before creating any metadata, check whether it already exists. If it does, update only if the spec has changed.
- **Deltas, not bulk.** When the user revises a phase brief mid-build, diff against current org and apply only the changes.
- **Permission sets, not profiles.** Grant access via permission sets. Do not modify standard profiles.
- **Test data only on request.** Sample/seed data loads only when the user explicitly approves.

## How scope is written (for your interpretation)

Scopezilla writes in two registers and tries not to mix them:

- **Business intent** — "Reps need a one-page meeting prep brief accessible from the account" — *your* job to map to the right Salesforce construct.
- **Real platform terms** — "CPQ explicitly out of scope," "native Quote object, not CPQ" — Scopezilla uses the genuine platform name when it knows the decision is platform-level. These are pre-decided.

If you encounter Salesforce-shaped language that doesn't match a real metadata type or feature (e.g., something that *sounds* like a feature name with custom labels stuck on it), treat it as business intent that was written too eagerly — translate to outcome and pick the platform construct yourself. Don't search for the literal feature.
